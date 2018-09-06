如果你曾经看到如 `K extends keyof T` 这样的代码，那么你可以阅读下去，看一看我们是如何去理解这样的事情。

```javascript
interface IPipe {
  name: string;
  id: number;
}

const o: IPipe = {
  name: "icepy",
  id: 0,
}

const t: IPipe = {
  name: "github",
  id: 10,
}

function getValues<T, K extends keyof T>(o: T, names: K[]): T[K][]{
  return names.map(n => o[n]);
}

console.log(getValues(t, ["name", "id"]));

// [ 'github', 10 ]
```

在这个例子中，我们想获取对象的 `values`, 从类型上来看我们并未知道传入和取出的类型，因此我们需要定义一个泛型函数，`keyof` 关键字可以从对象上获取到一组属性的联合类型，如上述的例子来看，我们可以假设传入的是 `t`，那么我们的泛型函数 `T` 实际上是 `IPipe` 类型，而 `K extends keyof T` 则是K属于T的继承，并且得到`Pipe`的属性联合类型 `"name" | "id" []`，因此我们从 `getValues` 函数上可以学习到，`T[K]` 其实就是 `Pipe[key]`，并且返回一个`(string | number)[]`的数组。

为什么编译器可以获取到值的类型呢？那是因为索引访问操作 `T[K]`，如果这个 `map` 访问到 `t[name]`，从 `IPipe` 的定义上来看 `Ipipe[name]` 是 `string`，因此 `T[K] = string `，这就是索引访问操作的强大之处，我们可以从这里理解为类型的索引，利用这个强大的特性，我们可以去完成很多很绕的事情，来声明我们的函数或类。

在入门基础中我们学习过联合类型的限制性，因此当我们要使用这个数组时，就需要进行类型保护式的处理，如：

```javascript
const ad = getValues(t, ["name", "id"]);

ad.forEach((v: string | number) => {
  if (typeof v === "string") {
    // string
  }

  if (typeof v === "number") {
    // number
  }
});
```

让我们再来看一个例子，官方定义的 `Readonly`，在 `React.Component` 中被用于 Props 的定义：

```javascript
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};

class Component<P>{
  constructor(props: Readonly<P>);
}
```

在这个场景中，我们能看到两个关键字 `in` 和 `keyof`，加速我们的实现为：

```javascript
interface IProps {
  name: string;
}

class EQ extends React.Component<IProps> {
  constructor(props:IProps) {
    super(props);
  }
}
```

由于 `in` 是遍历，而 `keyof` 是获取到属性的联合类型，并且我们传入的类型是 `IProps`，因此 `[P in keyof T]: T[P]` 分解的第一步就是：`[P in keyof IProps]: IProps[P]`，接下来 `keyof` 分解出 `("name")[]`，因此 `[P in keyof IProps]: IProps[P]` 又变成了 `[name]: IProps[name]`，然后我们给这里添加上 `readonly` 关键字，于是我们就能看到 `props` 都变成了只读。

## 总结

有时候我们会看见巨复杂的类型定义，不过这都没关系，只要我们能理解 `keyof` `T[K]` `in` 这些含义，我们可以仔细的分解它的逻辑，自然而然的就能明白这些巨复杂的类型究竟是什么。