在我们最基础的印象里 JavaScript 的对象都是引用关系，这对于我们的应用来说，可能会有无法预期的结果，比如：

```javascript
const f = {
  a: 1
}
function g(r) {
  r.a = 2
}
g(f)
console.log(f.a) // 2
```

这影响了外部定义的对象，有时候我们不可预知的错误，就是如此。那么，是否有更好的解决方案来解决这个对象不可变的事情呢？immutable 这个库就能很好的解决这个问题，虽然它总是出现在 React 的应用场景下，但它并不一定需要 React ，这是一个可以在其他环境中也可以运行的库。使用 immutable 创建的 Data ，一旦创建就不可变，任何的操作都会返回一个新的 Data，它的实现利用了 Persisten Data Structure 为我们提供了一个很不错的方案。

immutable.js 提供了各种的数据类型，比如 `Collection`，`List`，`Map`等等，其 API 也与我们熟悉的 Object 或 Array 的操作类似，这一点对于去理解它的人来说，非常的平滑。

让我们先来看一个例子：

```javascript
const newList = immutable.List([
  {
    id: "1",
    name: "xww",
  },
  {
    id: "2",
    name: "icepy",
  },
]);

const newMap = immutable.Map({id: "3", name: "ts"});
```

创建 immutable 对象就如上述一样的简单，现在，我们可以看一下渲染一个列表，如图：

![](../images/chap-04-07.png)

点击 `SET LIST DATA` 渲染一个列表

```javascript
public clickListHandler = () => {
  const newList = immutable.List([
    {
      id: "1",
      name: "xww",
    },
    {
      id: "2",
      name: "icepy",
    },
  ]);
  this.setState({
    list: newList,
  });
}

// render

<div>
  {
    list.map((v: IimmList | undefined) => {
      if (v) {
        return (
          <p key={v.id}>{v.name}</p>
        );
      }
      return null;
    })
  }
</div>
```

在 TypeScript 的世界里 List 是一个范型，需要指定一个类型，如：

```javascript
interface IimmList {
  id: string;
  name: string;
}

interface IState {
  list: immutable.List<IimmList>;
}
```

接下来，我们想改变第一个元素的值：

```javascript
public clickList0Handler = () => {
  const { list } = this.state;
  if (list.size > 0) {
    const list0 = list.get(0);
    if (list0.name === "xww-icepy") {
      return;
    }
    const newList = list.set(0, {
      id: "3",
      name: "xww-icepy",
    });
    this.setState({
      list: newList,
    });
  }
}
```

如图：

![](../images/chap-04-06.png)

其实，我们可以看到很多操作的 API 都非常的和 Array 像，并且和预期的结果一样，每一次的操作都会返回一个新的 immutable Data ，既然如此，我们还可以利用 React 本身的特性来进行优化，比如：

```javascript

public shouldComponentUpdate(nextProps: Readonly<{}>, nextState: Readonly<IState>, nextContext: any) {
  if (this.state.list !== nextState.list || immutable.is(this.state.list, nextState.list)) {
    return true;
  }
  return false;
}
```

immutable Data 的比较与我们以往的比较有些不一样，它比对的是 immutable Data 的 hashCode，如果我们的数据源嵌套很深，我们还可以使用游标（Cursor）来处理这个问题，这是一个很有趣的事情。

```javascript
import Immutable from 'immutable';
import Cursor from 'immutable/contrib/cursor';

let data = Immutable.fromJS({ a: { b: { c: 1 } } });
// 让 cursor 指向 { c: 1 }
let cursor = Cursor.from(data, ['a', 'b'], newData => {
  // 当 cursor 或其子 cursor 执行 update 时调用
  console.log(newData);
});

cursor.get('c'); // 1
cursor = cursor.update('c', x => x + 1);
cursor.get('c'); // 2
```

通过上述的一些知识点分享，我们应该对 immutable Data 有一个基础的认识，再结合 React 本身的特性，我们可以对于我们的应用程序做出优化的可能性。

如果你的数据使用 Redux 来驱动，搭配 immutable 稍微有些复杂，因为本身 Redux 对于数据的驱动有它自己的定义，比如 selec 来做检索，Action 来更新数据等等，两者的结合还是需要花费一番心思的。