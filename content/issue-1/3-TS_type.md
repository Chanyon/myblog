### 元组长度计算:

元组类型取length就是数值
example:

```
type num2 = [unknown,string]['length'] => 2
```

元组长度实现加减乘除

### 1. Add

```typescript
type BuildArray<
	Length extends number,
	Element = unknown,
	Arr extends unknown[] = [],
> = Arr["length"] extends Length
	? Arr
	: BuildArray<Length, Element, [...Arr, Element]>;

type Add<Num1 extends number, Num2 extends number> = 
  [...BuildArray<Num1>, ...BuildArray<Num2>]['length']

type AddRes = Add<1,1>
//值=> 2 
```

### 2. Subtract

```typescript
type Subtract<Num1 extends number, Num2 extends number> = 
  BuildArray<Num1> extends [...BuildArray<Num2>, ...infer Rest] ? 
  Rest['length'] : never;

type SubRes = Subtract<10,1>;
// 9
```

### 3. Multiply

乘法是多个加法累加的结果

```typescript
type Mul<Num1 extends number, Num2 extends number, ResultArray extends Array<unknown> = []> =
	Num2 extends 0 ? ResultArray['length'] :
  Mul<Num1,Subtract<Num2,1>, [...BuildArray<Num1>, ...ResultArray]>;

type MulRes = Mul<2,2>
// 4
```

### 4. Div

除法的实现就是被减数不断减去减数，直到减为 `0`

```typescript
//整除
type Div<Num1 extends number, Num2 extends number, ResultArray extends Array<unknown> = []> =
	Num1 extends 0 ? ResultArray['length'] :
	Div<Subtract<Num1,Num2>, Num2, [unknown,...ResultArray]>;

type DivRes = Div<2,1>
// 2
```
