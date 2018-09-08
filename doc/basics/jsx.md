如果你使用过 `React` 那么就应该对 `jsx` 非常的熟悉，这是一种嵌入式类似XML语法的可转换成 JavaScript 的设计实现，它非常的灵活在 JavaScript 中，我个人非常的喜欢 `jsx`，完全符合我自己的口味，菜好不好吃，只有自己去试试才知道。

TypeScript 也对于 `jsx` 有三种模式的支持，我们可以在 `tscnfig.json` 文件中找到：

```javascript
// "jsx": "preserve",  /* Specify JSX code generation: 'preserve', 'react-native', or 'react'. */
```

- `preserve`
- `react-native`
- `react`

要启用 `jsx` 在 TypeScript 的世界中需要做两件事情：

- 使用 `.tsx` 后缀的文件
- 在 `tsconfig.json` 配置文件中启用 `jsx` 选项

这三种模式下，各有不同。

- `preserve` 模式下对代码的编译会保留 `jsx` 格式，并输出一份 `.jsx` 后缀的文件。
- `react` 模式下对代码的编译是直接转换成 `React.createElement` ，并输出一份 `.js` 后缀的文件。
- `react-native` 模式下对代码的编译会保留 `jsx` 格式，并输出一份 `.js` 后缀的文件。

其他的使用方式几乎一样，并未有过多需要注意的地方，目前 TypeScript 3.0 已经支持了 `defaultProps` ，这也意味着我们可以很方便的定义默认值，不必像以前那样搞的很复杂，这一点上来说是新版本的 TypeScript 对 `react` 更好的支持。

## 范例

让我们来看一看一个简单的无状态函数式组件的定义：

```javascript

const TestPage: React.SFC = () => {
  return (
    <div>
      this is test page.
    </div>
  );
};

export default TestPage;
```

如果你要传递 Props ，那么可以如此：

```javascript

export interface IHeaderProps {
  localImageSrc: string;
  onLineImageSrc: string;
}

export const Header: React.SFC<IHeaderProps> = (props: IHeaderProps) => {
  const { localImageSrc, onLineImageSrc } = props;
  return (
    <div className={styles["header-container"]}>
      <img src={localImageSrc} />
      <img src={onLineImageSrc} />
    </div>
  );
};
```

我们也可以定义一个有状态的类组件，如：

```javascript
export interface IHomePageState {
  name: string;
}

export interface IHomePageProps {
  home: string;
}

class HomeComponent extends React.Component<IHomePageProps, IHomePageState> {
  constructor(props: IHomePageProps) {
    super(props);
    this.state = {
      name: "",
    };
  }

  public setName = () => {
    this.setState({
      name: "icepy",
    });
  }
  
  public render(){
    const { name } = this.state;
    const { home } = this.props;
    return (
      <div>
         <div onClick={this.setName}> set name </div>
         <div>{name} {home}</div>
      </div>
    )
  }
}
```

类型检查还是会和正常的 TypeScript 一样（`.ts` 后缀的文件名），因此我们不必过于担心。
