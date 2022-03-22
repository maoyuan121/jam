# jam-core-react


`jam-core` 是 [Jam](https://jam.systems) 的客户端 JavaScript/TypeScript 库。更对关于 Jam 的信息请见 [on our Gitlab page](https://gitlab.com/jam-systems/jam)。

`jam-core-react` 是一个帮助将 `jam-core` 集成到 React 应用中的包。
你可以使用它构建自定义的 Jam  UI。

```sh
npm i jam-core-react
```

## Usage

```js
import React, {useState, useEffect} from 'react';
import {render} from 'react-dom';
import {JamProvider, useJam, use} from 'jam-core-react';

const jamConfig = {domain: 'jam.systems'};

// Wrap your app in a Provider
render(
  <JamProvider options={{jamConfig}}>
    <App />
  </JamProvider>,
  document.querySelector('#root')
);

function App() {
  // get Jam state and API methods
  let [state, api] = useJam();

  // listen to specific state changes
  let [myIdentity, roomId] = use(state, ['myIdentity', 'roomId']);

  return (
    <div>
      User: {myIdentity.info.name ?? ''},<br />
      Room: {roomId ?? ''}
    </div>
  );
}
```

`state` 和 `api` 对象与 `jam-core` 中的 `createJam()` 返回的是一样的。 查看 [jam-core documentation](https://gitlab.com/jam-systems/jam/-/tree/master/ui/packages/jam-core) 获取关于如何使用它们的大量文档和示例。

与 React 集成的其他步骤很简单：

- 将你的应用包装在一个 `JamProvider` 中。这将为你调用 `createJam()`。
- 使用 `useJam()` 钩子来获取 Provider 中任何组件的 `state` 和 `api`
- 使用 `use(state, keys)` 钩子来更新组件，每当 `state` 中的 `keys` 发生变化时。

## Example App

我们有一个用 React 构建的简单 UI 的完整例子: https://gitlab.com/jam-systems/jam/-/blob/master/ui/examples/tiny-jam-react/App.jsx

在这个例子中，用户可以创建 Jam 房间，通过 URL 共享房间，并在同一个房间中相互交谈——只需不到 100 行代码。

这个例子可以在 https://tiny-jam-react.vercel.app 上看到