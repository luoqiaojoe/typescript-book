接口并未出现在 JavaScript 中，这应该属于 TypeScript 核心原则之一的设计，在 TypeScript 的世界里接口唯一的作用就是为你的代码或者第三方代码定义类型，约定好契约关系。

## 基础

当我们回过头来去看一个函数参数的对象扩展语法，如：

```javascript

function sum ({x, y}: { x: number, y: number}): number {
  return x + y;
}
```

试想一下，当对象的属性比较多时，代码就非常不适合阅读了，我们可以使用接口简化一下：

```javascript
interface ISum {
  x: number;
  y: number;
}

function sum ({ x, y }: ISum): number {
  return x + y;
}

```

这就是接口带来的收益；

当然，除此之外，接口的定义上有非常多丰富的范例，供你完成各式各样的编程场景。

### 可选属性

```javascript
interface ISumx {
  x: number;
  y?: number;
}

function xsum({x, y}: ISumx): number {
  return x;
}

xsum({ x: 0 });
xsum({ x: 0, y: 1});

```

### readonly

```javascript

interface IReadonlyPoint {
  readonly x: number;
  readonly y: number;
}

let p: IReadonlyPoint = { x: 0, y: 1};

// p.x = 1;
```

如果一旦赋值，编译器将提示一个错误。

### 属性检查

```javascript
interface ICheckPoint {
  x: number;
  y: number;
}

function create(config: ICheckPoint): void {

}

create({ xx: 0, x: 0, y: 1 } as ICheckPoint)
```

在 JavaScript 中这段代码并不会有错，因为对于对象当传进一个未知的属性时并不是错误，虽然可能会引发潜在的 bug。但这在 TypeScript 的世界中，这个错误是非常明显的，编译器并不会通过编译，除非你显式的使用类型断言；

### 继承

```javascript
interface IAb {
  a: string;
}

interface IBc extends IAb {
  b: string;
  c: string;
}
```

与类的继承使用了相同的关键字；

### 函数类型

```javascript
interface IFunc {
  sum: (x: number, y: number) => number;
}

const d: IFunc = {
  sum(x: number, y: number): number{
    return x + y;
  }
}
```

### 索引类型

```javascript
interface IArrayC {
  [index: number]: string;
}

const c: IArrayC = ["icepy", "hunan"];
c[0];
```

个人觉得索引类型最有用的地方是在使用 `o["k"]` 这样的方式来获取对象的属性或设置对象的属性时：

```javascript
interface IObjectO {
  [key: string]: number;
}

let o: IObjectO = Object.create(null);
let createId = function(){
  o[String(Math.random() * 100)] = Math.random() * 100;
}

createId();
console.log(o);
```

### 必须实现的接口

之前我们在写到抽象类时，做了一个简单的比喻，我们可以自己定义派生出来的类，要不要实现，其实在其他语言中接口也有同样的作用，只不过我们这里指的是实现具体的接口，如：

```javascript
interface ClockInterface {
  currentTime: Date;
  setTime(d: Date): void;
}

class Clock implements ClockInterface {
  currentTime: Date;
  constructor() { 
    this.currentTime = new Date();
  }
  setTime(d: Date) {
    this.currentTime = d;
  }
}
```

更多的案例，我们都可以通过此处学习 [http://www.typescriptlang.org/docs/handbook/interfaces.html](http://www.typescriptlang.org/docs/handbook/interfaces.html) 。

## 范例

这一部分的范例其实能说的会比较多，但往浅了说，我们所看到的运用方式，如上述一样，更具体的场景，可能就要在高级编程中来给大家讲解了。

完整的例子：[https://github.com/welearnmore/typescript-book-demos/blob/master/interface.ts](https://github.com/welearnmore/typescript-book-demos/blob/master/interface.ts)