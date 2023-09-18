
#### 1.简化版防抖函数
    指定时间内只执行一次回调函数

```javascript
function debounce(fn, wait = 2500) {
  let timer = undefined;
  return function (...args) {
    if (timer) {
      clearTimeout(timer);
    }
    timer = setTimeout(() => {
      fn.apply(this, args);
    }, wait);
  }
}

document.addEventListener("mousemove",
debounce((e) => console.log(1)), false);
```

#### 2.节流函数
    触发事件间隔大于等于指定的时间才会执行回调函数

```javascript
function throttle(fn, wait = 500) {
  let lastTime = 0;
  return function(...args) {
    const now = +new Date();

    if (now - lastTime > wait) {
      lastTime = now;

      fn.apply(this, args);
    }
  }
}

setInterval(throttle(() => console.log(2), 2000), 10);
```