之所以有这样的一篇文章写 `material-ui` 的一些高阶运用，那是因为我很喜欢 [material design](https://material.io/design/) ，而 `material-ui` 又刚好是一个全面遵循 `material design` 的 `React` UI 组件库，因此我将它写下来并推荐给大家。

对于普通的应用，毫无疑问我们需要通过 `npm` 来安装 `material-ui`：

```bash
$ npm i @material-ui/core --save
```

```javascript
import * as React from "react";
import Button from "@material-ui/core/Button";

export const renderButton: React.SFC = () => {
  return (
    <Button variant="outlined" color="primary" >1</Button>
  )
}
```

## Theme

我们可以创建一个 `Theme` 来影响本身 `material-ui` 的样式，如：

```javascript
// theme.ts

export default {
  overrides: {
    MuiButton: {
      ...
    }
  }
}

// xxx.tsx
import Home from "./home";
import { MuiThemeProvider, createMuiTheme } from "@material-ui/core/styles";
import customTheme from "./theme";

const theme = createMuiTheme(customTheme);

...
return (
  <Router>
    <MuiThemeProvider theme={theme}>
      <Route exact path="/" component={Home} />
    </MuiThemeProvider>
  </Router>
)
```

这个主题可以说是一个全局性的；

## withStyle

我们还可以通过一个高阶组件来修改局部的 `material-ui` 样式，如：

```javascript
import { withStyles, createStyles } from "@material-ui/core/styles";

const s = createStyles({});

class Main extends React.Component<IMainProps, IMainState> {
  public render(){
    const { classes } = this.props;
    return (
      <div></div>
    )
  }
}

export default withStyle(s)(Main);
```

在这种局部性的影响中，我们可以通过 `createStyle` 的方式来完成样式的创建，然后通过高阶组件传递给具体的组件。

## 样式命名自定义

在现在的前端世界中，对于打包这样的概念来说，我们都会分为开发和生产两个环境，由于 `material-ui` 定义了它开发和生产两个环境的样式名称，如果你想替换，那么我们需要使用到 `jss` 的另外一个高阶组件来处理这个事情，如：

```javascript
import { jssPreset, createGenerateClassName } from "@material-ui/core/styles";
import { create as createJss } from "jss";
import { JssProvider } from "react-jss";

const jss = createJss({plugins:  [...jssPreset().plugins]});
const generateClassName = createGenerateClassName({
  dangerouslyUseGlobalCSS: false,
  productionPrefix: "typescript-book",
});


....

<JssProvider jss={jss} generateClassName={generateClassName}>
  <div></div>
</JssProvider>,
```

此时，你可以在控制台中看一看最终的样式名称，这个地方的自定义主要处理防止冲突的问题。

更多有趣的使用，不妨阅读一下 [https://github.com/mui-org/material-ui](https://github.com/mui-org/material-ui) 。