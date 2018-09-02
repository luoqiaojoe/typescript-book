从 `es2015` 开始 JavaScript 有了自己的模块，TypeScript 也遵循了这样的定义，其实在 TypeScript 还有一个命名空间的概念，这样的概念应该说从历史遵循而来，上了年纪的前端程序员应该会对YUI有一些印象，当年的前端对于命名空间的运用，这是最出色的一款框架。不过，今天我们将讲一讲 TypeScript 中的模块，以及另外一些模块规范的第三方包，该如何在 TypeScript 中引用。

对于模块而言它自身是运行在一个局部的作用域中的，这一点非常重要，这也意味着在一个模块中的变量，函数，类等，除非你导出，外部程序是无法使用的，因此 TypeScript 也遵循了 `import` 和 `export` 的机制，反之如果一个文件内没有这两个关键字存在的话，那么它的内容将会被视为全局。

## 基础

导出一个常量：

```javascript

export const D = "";

```

导入一个常量：

```javascript

import { D } from "xx";

```

导出关键字和导入关键字都适用于重命名，如：

```javascript

import { D as Dell } from "xx"

const C = "";
export { C as D }

```

我们也可以将所有的导出都导入到一个变量中，如：


```javascript
export const A = "";
export const B = "";
```

```javascript
import * as CONST from "xx";
```

每一个模块都可以使用一个默认的导出，如：

```javascript
export default function reducers () {}
```

```javascript
import reducers from "xx";
```

## 范例

在目前的 Web 前端工程中，大部分情况下我们都会在 JavaScript 引入样式，如：

```javascript
import "./style.css"
```

当这样的范例有时候编译器并不能编译通过，也许我们可以定义一个通用的模块，如：

```javascript
declare module "*.css";
```

当然，你也可以使用 `require` 语句来导入一个样式文件。

由于在我们的工作中，会引用到很多第三方的模块，由于历史的原因，可能很多第三方模块，用了 `AMD` 或者 `commonjs`，或者它们压根就没有 TypeScript 的声明文件，重要的是我们都可以定义一个自己的描述文件，起码可以让程序 Run 起来。

举例：

`小明` 在解析一个 url 时用了一个第三方包来完成，但是当他将第三方包引入到工程中时发现报错了，于是他自己定义了一个模块声明来处理这个错误，如：

```javascript
declare module "url" {
  export function parse(urlStr: string) => string;
}
```

我们可以在工程中创建一个 `typing` 目录，编译器会自动的读取这个声明文件。

但是目前看起来工程中的错误，还是没有解决，因为它是一个 `commonjs` 的模块，由于 `commonjs` 中定义了 `module.exports` 和 `exports`，根据官方给出的建议是使用 `var d = require("xxx")`，但是对于有风格洁癖的人来说，这很不好看，但是我们可以从中遵循它的规则，由于 `exports` 的关系，以及现代编译器编译的规则，假设是导出了主模块，那么我们可以使用：

```javascript
import g from "xx";

g.default()

```

假设，我们的 `commonjs` 是使用的 `exports` ，那么我们可以使用：

```javascript
import * as g from "xx";

```

来解决这个问题。

于是 `小明` 很愉快的解决了错误，对于高效的程序员来说，老板又给 `小明` 涨薪了。

我们来看一看最终编译之后的代码：

```javascript
"use strict";
var __importDefault = (this && this.__importDefault) || function (mod) {
    return (mod && mod.__esModule) ? mod : { "default": mod };
};
Object.defineProperty(exports, "__esModule", { value: true });
const url_1 = __importDefault(require("browser-used/lib/url"));
console.log(url_1.default);
```

是不是发现了很多很有趣的地方。