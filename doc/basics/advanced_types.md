从英译的文字来看 `高级类型` 并未有我们想象的那么复杂，这只是对于我们日常的编程生活中的一些补充，某些场景下，这些类型会为你的编程范式带来便捷。

## 基础

当我们从最初的 `mixins` 中获取收益时，你就需要用到如下的一个类型了：

```javascript
function extend<T, U>(first: T, second: U): T & U {
  let result = <T & U>{};
  for (let id in first) {
    (<any>result)[id] = (<any>first)[id];
  }
  for (let id in second) {
    if (!result.hasOwnProperty(id)) {
        (<any>result)[id] = (<any>second)[id];
    }
  }
  return result;
}

class Person {
  constructor(public name: string) { }
}
interface Loggable {
  log(): void;
}
class ConsoleLogger implements Loggable {
  log() {
    // ...
  }
}
var jim = extend(new Person("Jim"), new ConsoleLogger());
var n = jim.name;
jim.log();
```

如果你使用过 vue 的话，应该体验过 `mixins` 带来的便捷性。

当我们写了一个函数，它的参数预期可能是 `number` 也可能是 `string` ，除了泛型的方式之外，我们也可以使用这样的方式来处理这个问题：

```javascript
function sumStr(x: number | string): string {
  if (typeof x === "number") {
    return `${x}`;
  }
  if (typeof x === "string") {
    return `${x}`;
  }
  throw new Error("not number or string");
}
```

从符号上来说 `x` 可能是 `number` 也可能是 `string` ，如果是其他类型，就抛出一个错误。

我们再来看一个稍微复杂一些的范例：

```javascript
class A {
  log(){
    console.log("A");
  }
}

class B {
  logg(){
    console.log("B");
  }
}

function getx(): A | B {
  return new A();
}

const x = getx();

(<A>x).log();
```

如果当你返回两个不同的类型时，为了让这段代码可以工作，我们需要断言一个明确的类型，然后才开始工作。

与 JavaScript 一样，我们都使用 `typeof` 和 `instanceof` 来做类型保护，用于判断当一个变量是未知时的类型判断。这些预期的行为与 JavaScript 没有任何区别，因此我们应该去更好的理解 `instanceof` 来处理对象。

大部分情况下，如果没有使用接口来定义类型，我们为会比较杂乱的类型定义一个别名，这对于程序的可阅读性有较高的提升，如：

```javascript

type info = {
  name: string;
  age: number;
  x: string;
}

let a: info = {
  name: "",
  age: 0,
  x: ""
}

let b: {
  name: string;
  age: number;
  x: string;
} = {
  name: "",
  age: 0,
  x: ""
}
```

你能所预期的，如果没有别名，这些不是接口定义的类型，随着属性的增加会恐怖到什么地步。既然我们提到了接口，那么有一些不一样的地方，那是它与别名的区别，别名不能像接口那样被继承和被实现。 

## 范例

`小明` 在公司的业务中又遇到了一个难题，是什么呢？他想做一个比较通用的动画参数，虽然这可以用常量，或者枚举来完成，但是由于需要被输入，并且限定传入参数的值，因此 `小明` 想到了字符串字面量，如：

```javascript
type Easing = "ease-in" | "ease-out" | "ease-in-out";
function animation(dur: number, easing: Easing){
  if (easing === "ease-in") {

  } else if (easing === "ease-out") {

  } else if (easing === "ease-in-out") {

  } else {
    throw new Error("animation easing error");
  }
}
```

通过这个字符串字面量，我们可以很容易的在编译器层面就知道输入的值到底是不是我们想要的。

advanced types 还有许多很有趣的使用方式，详细请参考 [http://www.typescriptlang.org/docs/handbook/advanced-types.html](http://www.typescriptlang.org/docs/handbook/advanced-types.html) ，但大体上上述的一些高级类型使用，是会比较常见的。