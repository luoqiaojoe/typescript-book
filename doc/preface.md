传统意义上的 JavaScript 是一个非常松散的语言，因此在以往的面试过程中，很大几率会问及类型以及一些区别。但更传统的软件工程中我们常常在使用的是由编译器进行过编译的语言（转换成了机器实际可运行的格式），由于编译阶段的存在，我们可以更易于找到错误和进行优化（想想 JavaScript 为什么要总结那么多的编程范例，""+ "" 和 [].join() 的区别）。虽然看起来 TypeScript 就是一个增强 JavaScript 的工具，但是它也可以成为启用其他工具的工具，最简单的比如：抽象语法树（AST）。

做为《浅谈 TypeScript》 小书的开篇前言，我们不会讲到编译的过程，反而我们这本小书更注重 TypeScript 本身的运用，根据国际惯例，我们还是会讲到一些介绍性的内容，比如：

- TypeScript 是什么
- 使用 TypeScript 的收益是什么
- 如何书写一段简单的 TypeScript 例子


## TypeScript 是什么

我们需要认准官方网站的入口：[http://www.typescriptlang.org/](http://www.typescriptlang.org/)，并引用一段 `官网的介绍`:

> TypeScript is a typed superset of JavaScript that compiles to plain JavaScript. Any browser. Any Host. Any OS. Open source.

其实在这里，你就能很明白 TypeScript 是什么了，做为 JavaScript 的超集，它添加了非常强健的静态类型系统，可以用于 Web 网页，也可以用于 Node.js，只要是 JavaScript 可以运行的地方，它都可以运行。

## 使用 TypeScript 的收益是什么

举个简单的例子，`小明` 公司的业务有超过十万+的 JavaScript 代码和 100+ 的接口，他们公司的后端程序员 `小李` 根据公司业务的发展需求，变更了一些接口和字段，由于 JavaScript 没有编译阶段的类型检查，`小明` 根据前人留下的文档和记忆，只能一个文件一个文件的修改，并且在心里默默骂道：`真坑`，当 `小明` 加班了一个晚上看起来改的差不多了，由于老板催的急，于是发布上线，第二天 `小李` 表示就再也没有在公司见到过 `小明`。当我们有了 TypeScript 之后改怎么办呢？由于我们的接口字段已经定义了 `interface` ，因此 `小明` 只需要将定义的接口中的某个字段改个名字，如果你使用了 `VSCode` ，有依赖这个接口的所有代码都将标记一个红色的反馈。（当然，你也可以编译一次） ，于是 `小明` 愉快的在没有前人文档的情况下，正确的发布上线了，第二天 `小李` 发现 `小明` 涨薪了。 

言归正传，当我们使用 TypeScript 时，也许能收获如下收益：

- 强大的静态类型系统
- 完善的内部代码库
- 几乎所有的 API 都能得到准确的智能提示
- 正统的编程范式
- 可以帮助你的团队更好的理解架构内部的数据流向

说了这么多，其实我个人的看法是我们可以更明确的输出文档式的接口声明，这在前端的应用中非常的有用，就如我开玩笑式的举例也许有一天你将面临同样的问题。反而当我们使用类型数量（TypeScript）的增加，我们迟早会从中获益，并且 TypeScript 还会去理解你的代码，并为你的工作提供更好的支持。

## 范例

```bash
$ npm i -g typescript
```

TypeScript 的编译器可以全局安装也可以局部安装，它是一个很有用的 Node.js 模块，当我们全局安装它时，我们就能在命令行中通过 `tsc` 来运行它。

创建一个 `hello.ts` 文件，并写入：

```javascript

const hello: string = 0;

```

运行一下 `tsc hello.ts` 看一看效果，如图：

![](./images/chap-01-01.png)


当我们声明了一个字符串的常量，但却赋值了一个数值，于是在编译过程中，就得到了一个错误，这是 TypeScript 教学式的运用。当我们将 TypeScript 引用在真实的项目中时，肯定就不会这么来用了。

现代的 Web 前端工程几乎都用到了构建工具，如 `webpack`：

```bash
npm i awesome-typescript-loade typescript source-map-loader --dev
```

假设你使用了 `React` 来编写你的应用，那么我们就应该如下的编写你的构建脚本：

```javascript

var fs = require('fs')
var path = require('path')
var webpack = require('webpack')
const { CheckerPlugin } = require('awesome-typescript-loader');
var ROOT = path.resolve(__dirname)

module.exports = {
  entry: './src/index.tsx',
  devtool: 'source-map',
  output: {
    path: ROOT + '/dist',
    filename: '[name].bundle.js',
    sourceMapFilename: '[name].bundle.map.js',
    publicPath: '//localhost:8889/dist/',
  },
  devServer: {
    inline: true,
    quiet: true,
    contentBase: "./",
    port: 8889
  },
  module: {
    rules: [
      { test: /\.ts[x]?$/, loader: "awesome-typescript-loader" },
      { enforce: "pre", test: /\.ts[x]$/, loader: "source-map-loader" },
    ]
  },
  resolve: {
    extensions: [".ts", ".tsx"],
    alias: {
      '@': path.resolve(ROOT,'src')
    }
  },
  plugins: [
    new CheckerPlugin(),
  ]
}
```

更多的时候工程化的范式可以减少我们很多的工作量。