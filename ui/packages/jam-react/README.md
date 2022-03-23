<p align="center">
  <img title='Jam mascot by @eejitlikeme'
       src="https://jam.systems/img/jam.jpg"
       width="300"
       height="300"/>
</p>

# jam-react

Jam 是您自己的嵌入式 Clubhouse 式音频空间。这个包将 Jam 公开为一个独立的 React 组件。

```sh
yarn add jam-react
```

用法：

```js
import Jam from 'jam-react';

<Jam roomId="new-jam-room" />;
```

`<Jam>` 支持 3 个 props：

- `roomId` - **必填**, 字符串, 您的房间 ID，也使房间可通过导航到 `https://jam.systems/<roomId>` 进行访问。
- `jamUrl` - 可选, 字符串, jam 实例的 URL。默认是 `https://jam.systems`，你可以任意指定。
- `params` - 可选, 允许许多自定义的文档 [here](https://gitlab.com/jam-systems/jam#room-configuration-via-url).

任何其他 props，如 `style`，会合并到加载 jam 的顶层 `<iframe>`。这允许你添加样式，自定义 `iframe` 特性策略等。注意 Jam 需要 `allow="microphone"` 才能工作。

下面是一个完整的自定义示例，包括从外部为用户设置标识:

```js
import React from 'react';
import Jam from 'jam-react';

function App() {
  return (
    <div style={{padding: '1rem'}}>
      <h1>Jam: My own Clubhouse!!!!</h1>
      <Jam
        jamUrl="http://beta.jam.systems"
        roomId="klubhaus-123456"
        params={{
          room: {
            name: 'A new Jam Room',
            description: 'This Room was created by a React component',
            color: '#000000',
            stageOnly: true,
          },
          ux: {
            autoCreate: false,
            autoJoin: true,
          },
          identity: {
            name: 'Gregor',
            avatar: 'https://avatars.githubusercontent.com/u/20989968',
          },
        }}
        style={{width: '400px', height: '600px', border: 'none'}}
      />
    </div>
  );
}
```

下面是一个例子，三个 `<Jam>` 组件并排呈现:

<p align="center">
  <img src="https://i.imgur.com/nmYENw9.png"
       width="900"/>
</p>

## About Jam

🍞 Jam 是 Clubhouse、Twitter Spaces 和类似音频空间的开源替代品。

使用Jam，你可以创建音频室，可以用于小组讨论、Jam 会议、自由流动的对话、辩论、戏剧、音乐剧等。唯一的限制就是你的想象力。


 在 [https://jam.systems/](https://jam.systems/) 试用 Jam

在 [Gitlab repository](https://gitlab.com/jam-systems/jam/) 查看更多关于 Jam 的信息

## Buy Us ☕

**BTC:** 3HM1zPtLuwCGarbihNYVjFVwbFrFe9keqh

**ETH:** 0xe15265b2a309f0d20038e10b8df5a12fb5e916f8
