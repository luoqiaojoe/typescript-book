TypeScript 支持与 JavaScript 几乎相同的数据类型，只不过它会比 JavaScript 更为丰富，详情参考[http://www.typescriptlang.org/docs/handbook/basic-types.html](http://www.typescriptlang.org/docs/handbook/basic-types.html)。

在 TypeScript 的世界中因为推导的关系，我们可以不必明确给予类型，因为编译器会通过值来推导类型，大部分情况下这都是有用的，不过既然这是一个静态类型系统，我们更期望在书写的过程中能有非常明确的类型。

## 基础

```javascript

let is: boolean = true;
let isN: number = 110;
let isS: string = "";
let isA: Array<any> = [];
let isNull: null = null;
let isUndefined: undefined = undefined;
```

上述的类型基本上 JavaScript 中都能找到对应的含义：

- 布尔值
- 数值
- 字符串
- 数组
- null
- undefined

其中数组还有一种简写，如：`let isA: any[] = []`。除此之外，TypeScript 在基本类型上扩充了一些其他非常有用的类型：

```javascript

let x: [string, number] = ["hello", 110]; 
enum Color {
  Red,
  Green,
  Blue
}
let Ans: any = 4;
let Ans: any = "4";
let unusable: void = undefined;
function error(msg: string) never {
  throw new Error(msg);
}
```

- 元组
- 枚举
- Any
- Void
- Never

多数情况下，`元组` 的使用倒是可以在函数返回多个值时有用，一般情况下，我们可能会使用不到它。而 `Never` 来表示没有类型，也就是几乎在栈错误中去使用。`Any` 的情况是在未知类型的时候使用，不过程序一般都有比较明确脉络走向，我们可以使用泛型来代替 Any 去处理这样的状况，至于 `Void` 一般也是用于定义函数没有返回值时所用。

在 TypeScript 扩充的这些基本类型中，比较常用的还是枚举，它不仅可以使用数字枚举，也可以字符串枚举，和其他语言中的枚举一样，它的值都有着明确特殊含义的，与你的编程逻辑密不可分。

```javascript
enum Color {
  Red = "red",
  Green = "green"
}

const typed = document.getElementById("typed");
if (typed) {
  typed.style.background = Color.Red;
}
```

如图：

![](../images/chap-02-01.png)

在上述的文字中，其实你应该可以发现存在 `let` 关键字，接下来它与我想说的变量声明有非常大的关系。在以往的 JavaScript 中 `var` 是我们来定义一个变量的开始，es2015 普及之后，`let` 和 `const` 几乎就成了我们编写代码时仅选择的两个变量声明方式，虽然某些特殊的场景下，我们也会使用到 `var`。

说到变量的声明，其实要说到作用域的概念，这有一些比较枯燥的理论知识，所以简单的来说，不管是 `let` 还是 `const` 它的作用域都是块作用域，一个是可以在块作用域中随意赋值的 `let`，一个是一旦赋值就不可改变的 `const `。

何谓`块`，简单的理解可以为只要是 `{}` 包裹的区域，它就是一个块，在这个块中变量的声明是有一定规则的，最主要的规则就是：**在块中声明的变量是无法被块作用域外的变量所访问**，更多的理论知识可参考 [http://www.typescriptlang.org/docs/handbook/variable-declarations.html](http://www.typescriptlang.org/docs/handbook/variable-declarations.html) 。

说到这里，其实我想说一说我们在写 es2015 时非常有用的一个特性：解构。在 TypeScript 中解构也是非常有用的一个特性：

```javascript
let input = [1,2]
let [f, s] = input; // 数组解构
```

```javascript

function a([f,s]:[number,number]){} // 用于函数的数组解构

a([1,2])
```

```javascript

let input = [1,2,3,4];
let [f, ...s] = input;
//s [2,3,4] ...展开语法
```

```javascript

let o = { a: 1, b: 2}

let { a, b} = o; // 对象解构
```

正常情况下，我们非常希望在对象的解构过程中给予一些类型，简单来说我们应该通过一个 `interface` 来定义一个对象，上述的情况也可以将类型赋值上去，只是代码看起来不是很满意，如：

```javascript
let { a, b }: {a:number, b: number } = o;
```

## 范例

举个例子：`小李` 在公司写代码，为了省事情，他都喜欢用解构的方式来写 Action，如：

```javascript

type addType = { address: string; phone: number};
export const add = ({ address, phone}: addType) => (dispatch: Dispatch) =>  {
  //...
}

// React 伪代码

add() {
  this.props.add(this.state.home);
}

```


举个例子：`小明` 的公司有一个业务时去做一个封面的书页颜色，由于这个需求可能会在后期的业务发展中而改变，如果是简单一些的方式，我们可以将颜色非常容易的写死，或者是书写多个常量去描述颜色，但这很分散，于是我们可以将颜色定义一个字符串的枚举，如：

```javascript

enum Color {
  Red = "red",
  Green = "green",
  Blue = "blue"
}

dom.style.background = Color.Blue;
```

如果你也使用着 Redux ，我们也可以将枚举用于非常具体的 Action 中，我们完全可以在 reducers 里通过枚举来处理数据流向，如：

```javascript
enum Action {
  add = 0,
  update,
  remove,
  sync
}

interface IStoreAction {
  type: number;
  payload: any;
}

interface IStoreState {
  id: string;
}

export function reducers(state={}, action: IStoreAction): IStoreState {
  const { type, payload } = action;
  switch (type) {
    case Action.add:

      break;
    case Action.update:

      break;
    ...
  }
}

```