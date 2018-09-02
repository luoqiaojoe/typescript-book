在没有出现 `es2015` 之前在 JavaScript 中，我们都使用函数和原型来完成一个类的定义，但这对于熟悉其他面向对象的程序员（Java）来说非常的艰难，于是 `es2015` 将我们复杂的面向对象编程简化了不少，反而 TypeScript 对于它还有一些增强的扩展，我们可以一直使用它而不必等待下一版本的 JavaScript 标准实现，并且这和标准非常的类似。

## 基础

让我们来学习一个非常简单的类定义：

```javascript

class World {
  public country: string;
  private max: number;
  constructor(country: string){
    this.country = country;
    this.max = 110;
  }

  output() {
    return this.country;
  }
}

class Country extends World {
  constructor(country: string){
    super(country);
  }
}
```

在这个范例中，我们既定义了类也完成了继承，看起来是不是和 `es2015` 几乎一样呢？当然它也有一些与 `es2015` 的标准稍有不同，因为 TypeScript 增强了关于 `public` `private` 之类的一些定义，这些定义可以在下一个 JavaScript 版本得到了实现，所以我们可以无需关心而正常的使用它。

### 修饰符

- public 外部程序可以自由的访问
- private 外部程序不可以自由的访问
- protected 与 `private` 类似，唯一的不同是它可以在派生类中自由的访问
- readonly 与词同意，只读
- static 静态属性或方法

```javascript

class StaticClass {
  static orig = "";
  static origFun = () => {}
}

```

### 存取器

如果你使用过 `Object.defineProperty` 那么一定理解如 `getter` `setter` 钩子的作用，在 TypeScript 的类中也定义了非常类似的东西，举个例子：

```javascript

class Employee {
  private _fullName: string;

  constructor(){
    this._fullName = "";
  }
  
  get fullName(): string {
    return this._fullName;
  }

  set fullName(newValue: string) {
    this._fullName = newValue;
  }
}

const emp = new Employee();
emp.fullName = "";
```

比对一下编译之后的代码：

```javascript
var Employee = /** @class */ (function () {
  function Employee() {
    this._fullName = "";
  }
  Object.defineProperty(Employee.prototype, "fullName", {
    get: function () {
        return this._fullName;
    },
    set: function (newValue) {
        this._fullName = newValue;
    },
    enumerable: true,
    configurable: true
  });
  return Employee;
}());
var emp = new Employee();
emp.fullName = "";
```

是不是发现了 `Object.defineProperty` ？ 在使用存取器的过程中，唯一要注意的是如果只定义了 `get` 钩子而没有定义 `set` 钩子的话，这个属性将是 `readonly`。

### 抽象类

这个概念几乎在 JavaScript 从未存在过，如果有其他面向对象编程经验的程序应该会比较明白，说一个非常简单类似的设计，如iOS 中的 protocol：

```javascript

abstract class Department {
  constructor(public name: string) {
  }

  printName(): void {
    console.log('Department name: ' + this.name);
  }

  abstract printMeeting(): void; // 必须在派生类中实现
}

class AccountingDepartment extends Department {

  constructor() {
    super('Accounting and Auditing'); // 在派生类的构造函数中必须调用 super()
  }

  printMeeting(): void {
    console.log('The Accounting Department meets each Monday at 10am.');
  }

  generateReports(): void {
    console.log('Generating accounting reports...');
  }
}

```

## 范例

`小明` 最近很认真的在学习 TypeScript ，是什么给了他那么大的动力？如第一章里写的因为 `涨薪`，今天他在学习如何用一个类来操作 DOM 元素，对于此，他是这样设计的：

- 定义了一个 XDOM 类
- 定义了一个 inster 设置一个文本值
- 定义了一个 dom 属性
- 定义了一个 静态单例方法
- 定义了一个 setColor 的实例方法

```javascript
class XDom {
  static xdom: XDom | null;
  static shareInstance = (): XDom => {
    if (!XDom.xdom) {
      XDom.xdom = new XDom();
    }
    return XDom.xdom;
  }

  public dom: HTMLElement | null;
  constructor(){
    this.dom = document.getElementById("class");
  }

  inster(text: string){
    if (this.dom) {
      this.dom.innerHTML = text;
    }
  }

  setColor(color: string) {
    if (this.dom) {
      this.dom.style.color = color;
    }
  }
}

const xdom = XDom.shareInstance();
xdom.inster("icepy");
xdom.setColor("red");
```

如图:

![](../images/chap-02-04.png)

完整的例子：[https://github.com/welearnmore/typescript-book-demos/blob/master/class.ts](https://github.com/welearnmore/typescript-book-demos/blob/master/class.ts)