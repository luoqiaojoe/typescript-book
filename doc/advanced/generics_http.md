不知不觉冬去春来，`小明` 也换了一份工作，这次他在新的公司里开始负责设计前端项目，一切都从新的开始。有一天 `小明` 和 公司的后端程序员`小李` 愉快的对完了接口文档，于是开始埋头苦干中。由于请求的异步性，`小明` 随着知识学习的增长也开始了使用 `Promise`，他对于接口的约定，在实现上也会去考虑复用性，于是 `小明` 和 `小李` 完成了一项非常重要的约定：

```javascript

{
  code: 200,
  message: "",
  data: {}
}

```

所有的接口都从这样的格式开始，`code` 代表接口的成功与失败，`message` 代表接口失败之后的服务端消息输出，`data` 代表接口成功之后真正的逻辑，一期一共有三个接口，分别是：

- 登录
- 获取列表
- 退出

`小明` 使用泛型定义了一个 `response` 的类型，如：

```javascript

export enum StateCode {
  error = 400,
  ok = 200,
  timeout = 408,
  serviceError = 500
}

export interface IResponse<T = any> {
  code: StateCode;
  message: string;
  data: T;
}
```

接着 `小明` 实现了另外三个接口，这是与后端程序员 `小李` 一起约定好的，如：

```javascript
export interface ISignOut {
  signOut: boolean;
}

export interface ISignIn{
  Id: string;
  name: string;
  avatar: string;
  permissions: number[];
}

export interface ICommodity {
  id: string;
  img: string;
  name: string;
  price: number;
  unit: string;
}

export interface IFavorites {
  id: string;
  img: string;
  name: string;
  url: string;
}

export interface IList {
  commodity: ICommodity[];
  groups: IFavorites[];
}
```

由于已经约定好了接口统一使用 `POST`，Path 也有固定的地址，因此接下来 `小明` 利用一个开源请求库 `axios` 编写了一个比较通用的HTTP函数用于项目中：

```javascript
const ROOT = "https://icepy.me"

export function service({ path, data}: IRequest): Promise<IResponse>{
  return new Promise((resolve) => {
    const request: AxiosRequestConfig = {
      url: `${ROOT}/${path}`,
      method: "POST",
      data: data
    }
    axios(request).then((response: AxiosResponse<IResponse>) => {
      resolve(response.data);
    })
  });
}
```

`小明` 休息了会儿，然后大概花了五分钟左右的时间实现了三个接口的业务逻辑：

```javascript

// 登录
service({
  path: "/signin",
  data: {
    username: "icepy",
    password: "icepy"
  }
}).then((response: IResponse<ISignIn>) => {
  const { code, message, data} = response;
  if (code === StateCode.ok) {
    // 处理 
  }
});

// 退出
service({
  path: "/signout",
  data: {
    id:  "xxx"
  }
}).then((response: IResponse<ISignOut>) => {
  const { code } = response;
  if (code === StateCode.ok) {
    window.location.reload();
  }
})

// 获取列表

service({
  path: "/list",
  data: {
    id: "xxx"
  }
}).then((response: IResponse<IList>) => {
  const { code, data } = response;
  if (code === StateCode.ok) {
    data.commodity.map((v: ICommodity) => {
      
    });
  }
})
```

每一个接口的实现，我们都可以从约定的类型中得到 TypeScript 工具的支持，如图：

![](../images/chap-03-01.png)

![](../images/chap-03-02.png)


看了看表，才 `6点半`，于是 `小明` 愉快的下班去请女朋友吃饭了，第二天后端程序员 `小李` 表示很羡慕。