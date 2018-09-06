这篇文章会介绍一些如何优雅的使用 TypeScript 的奇技淫巧，如果你也有更多的使用技巧不妨评论留言，持续更新；

## 可选型

如果你能看见 `?` 那么在 TypeScript 的世界中，它的表述是可能也许，因此在处理这段逻辑时需要用到 `if`，如：

```javascript
function numS(s?: number) {
  if (s) {
    return s * 0;
  }
  return 0;
}

numS(0)
```

但是，某些情况下，我们可能不需要如此，假设这段函数就是因为如果 `s` 不是 `number` 时可以 `throw` 一个错误，因此，我们可以这样：

```javascript
function v(s?: number){
  if (!s){
    throw new Error("");
  }
}

function numS(s?: number) {
  v(s);
  return s! * 0;
}
```

如果可以统一，那么看起来就没有那么多的 `if` 了。

## Rest 参数

有时候我们可能需要批量的来获取参数，并且每一个参数的类型还不一样，我们可以声明一个元组如：

```javascript
function query(...args:[string, number, boolean]){
  const d: string = args[0];
  const n: number = args[1];
  const b: boolean = args[2];
}
```

## 多参数返回

如果有一天我们遇到了需要返回多个不同类型的参数时：

```javascript
function search(): [string, number] {
  return ["1", 1];
}

const {...args} = search();

args[0]
args[1]
```

## 另一种 Readonly

如果我们直接使用 `readonly` ，编译之后会增加很多其他的代码，如果我们利用 Class 的 get 钩子，也完全可以实现相同的场景，如：

```javascript
class QueryP {
  public _g?: string;
  get g(){
    return this._g;
  }
}

const queryp = new QueryP();

// queryp.g = "1234";
```

如果我们对 `g` 赋值，编译器会给出如下的错误：

```bash
skill.ts:75:8 - error TS2540: Cannot assign to 'g' because it is a constant or a read-only property.

75 queryp.g = "1234";
```

如果你的 `target` 是 `ES5` ，那么你应该能想到 `Object.defineProperty`。

## 推导对象Map的值

```javascript
function pluck<T, K extends keyof T>(o: T, names: K[]): T[K][] {
  return names.map(n => o[n]);
}

const d = pluck({ 
  d:"123"
}, ["d"]);
```


