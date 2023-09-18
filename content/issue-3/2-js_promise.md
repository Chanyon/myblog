### JavaScript Promise 简单实现

#### V1:
```js
class SimplePromise {
  constructor(executor) {
    this._status = 'pending'
    this._value = undefined
    this._reason = undefined
    
    const resolve = value => {
      if (this.status === 'pending') {
        this.status = 'fulfilled'
        this.value = value
      }
    }

    const reject = reason => {
      if (this.status === 'pending') {
        this.status = 'rejected'
        this.reason = reason
      }
    }

    try {
      executor(resolve, reject)
    } catch (error) {
      reject(error)
    }
  }

  then(onFulfilled, onRejected) {
    if (this.status === 'fulfilled') {
      onFulfilled(this.value)
    }
    if (this.status === 'rejected') {
      onRejected(this.reason)
    }
  }
}

const sp = new SimplePromise((resolve, reject) => {
  if (!true) {
    resolve(1)
  } else {
    reject("error")
  }
}).then((v) => { console.log(v) }, (e) => { console.log(e)})
```

#### V2:
```js
class SimplePromise {
  constructor(executor) {
    this._status = 'pending'
    this._value = undefined
    this._reason = undefined
    this._onFulfilledCallbacks = []
    this._onRejectedCallbacks = []
    
    const resolve = value => {
      if (value instanceof SimplePromise) {
        return value.then(resolve, reject)
      }
      setTimeout(() => {
        if (this._status === 'pending') {
          this._status = 'fulfilled'
          this._value = value
          this._onFulfilledCallbacks.forEach(cb => cb(this._value))
        }
      }, 0)
    }

    const reject = reason => {
      setTimeout(() => {
        if (this._status === 'pending') {
          this._status = 'rejected'
          this._reason = reason
          this._onRejectedCallbacks.forEach(cb => cb(this._reason))
        }
      }, 0)
    }

    try {
      executor(resolve, reject)
    } catch (error) {
      reject(error)
    }
  }

  then(onFulfilled, onRejected) {
    if (this._status === 'fulfilled') {
      setTimeout(() => {
        onFulfilled(this._value)
      }, 0);
    } else if (this._status === 'rejected') {
      setTimeout(() => {
        onRejected(this._reason)
      }, 0);
    } else if (this._status === 'pending') {
      this._onFulfilledCallbacks.push(onFulfilled)
      this._onRejectedCallbacks.push(onRejected)
    }

    return this
  }
}

const sp = new SimplePromise((resolve, rejected) => {
  // setTimeout(resolve, 1000, Math.random())
  resolve(new SimplePromise((resolve) => {
    setTimeout(resolve, 1000, Math.random())
  }))
})

setTimeout(() => {
  sp.then(v => console.log(`inner: ${v}`))
}, 0)
sp.then((v) => { console.log(`outer: ${v}`) }, (e) => { console.log(e) })
```