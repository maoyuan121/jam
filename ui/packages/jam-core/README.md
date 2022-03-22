# jam-core


 [🍓 Jam](https://jam.systems) 的客户端 JavaScript/TypeScript 库。

```sh
npm i jam-core
```

```js
import {createJam} from 'jam-core';

let roomId = 'my-jam-1234';
let jamConfig = {domain: 'jam.systems'};

// 创建客户端 jam 实例
const [state, {setProps, enterRoom, createRoom}] = createJam({jamConfig});

// 在 server  上创建 jam 房间
createRoom(roomId, {name: 'My Jam', stageOnly: true});

// set room id (= connect to the room from the client)
setProps({roomId});

// 加入房间。这将要求用户提供他们的麦克风，使他们能够听到其他人
await enterRoom(roomId);

console.log(state.inRoom === roomId); // true
```

将这个例子嵌入[在 HTML 页面中](https://gitlab.com/jam-systems/jam/-/blob/master/ui/examples/minimal-jam/index.html)，并在两个不同的浏览器中运行，这就足以让两个人通过 WebRTC 🎧 互相交谈

你可以使用 `jam-core` 为在浏览器中运行的 Jam 创建自定义 ui 和 bot。在我们的 Gitlab 页面上了解更多关于 Jam 的信息:[https://gitlab.com/jam-systems/jam](https://gitlab.com/jam-systems/jam)

`jam-core` 兼容 [Jam as a Service](https://jamshelf.com/) 在这我们为你们管理 Jam 服务器。这让你可以构建强大的连接音频应用，而只需要担心UI 🚀

`jam-core` 独立于任何 JavaScript/TypeScript 框架。你可能想看看配套的 库[jam-core-react](https://gitlab.com/jam-systems/jam/-/tree/master/ui/packages/jam-core-react)，它将 `jam-core` 集成到 React 应用程序中。

_这个库适用于所有现代浏览器。_

## Documentation

```js
import {createJam} from 'jam-core';
```

该 API 本质上由一个函数 `createJam()` 组成，它返回两个对象的数组:

```js
const [state, api] = createJam(options);
```

`state` 和 `api` 一起代表一个单独的用户在一个单独的 Jam 房间中（可以切换），就像在[Jam官方应用程序](https://jam.systems)。如果这是你的用例，你可能只需要在开始时调用 `createJam()` 一次。如果你想让你的页面作为多个用户，或在多个 Jam 房间在同一时间，调用 `createJam()` 多次。

`options` 参数主要用来指定 Jam 所在的域，以及该域是否启用了 SFU （服务器端流媒体）:

```js
let options = {
  // main options
  jamConfig: {
    domain: 'jam.example.com', // default: 'jam.systems', our public Jam instance
    sfu: false, // default: false
  },

  // other options you probably don't need:
  cachedRooms: {
    // cached rooms, to avoid the GET request which pulls room info from the server
    'my-hardcoded-room': {
      name: 'My hardcoded Room',
      speakers: [],
      moderators: [],
    },
  },
  debug: false, // log debug output to the console (don't set to true in production)
};
```

### `state`

`state` 包含当前客户端状态、房间、已发现的对等点和各种其他信息。`state` 仅用于读取——随着应用程序的发展，它将被 Jam 更改。

举个例子，你的 `state` 上的一个属性是 `state.myIdentity`，它存储在浏览器的 localStorage 中，如果没有找到，会自动创建：

```js
const [state, api] = createJam();

console.log(state.myIdentity);
// {
//   "publicKey": "3EQBODw0NzYHTfJhsBuf1M1wggPIHbWM2mZgz2DWQpU",
//   "secretKey": "wpGWxQYIDkdKivt4CKecSHIGxMkWZIsk9Y_T2wmUA3ncRAE4PDQ3NgdN8mGwG5_UzXCCA8gdtYzaZmDPYNZClQ",
//   "info": {
//     "id": "3EQBODw0NzYHTfJhsBuf1M1wggPIHbWM2mZgz2DWQpU"
//   }
// }
```

这是一个表示用户帐户的密码密匙对。公钥同时充当您的用户 id;它可以作为 `state.myId` 的快捷方式使用。


```js
console.log(state.myId); // "3EQBODw0NzYHTfJhsBuf1M1wggPIHbWM2mZgz2DWQpU"
```

更多例子：

```js
let {
  roomId, // string or null; the current room ID
  inRoom, // string or null; equal to roomId if the room was joined, null otherwise
  room, // = {name, description, speakers, moderators, stageOnly, ...}; various information about the room
  iAmSpeaker, // boolean; is the user a speaker in the room?

  peers, // 当前在同一个房间里面的用户的 id 数组
  identities, // = {peerId: {name, avatar}, peerId2: ...}; identity infos for the other peers

  myAudio, // the user's audio stream, can come from his mic or from a streamed file, or be null
  micMuted, // boolean; 用户是否静音了麦克风？

  peerState, // = {peerId: {micMuted, inRoom}, peerId2: ...}; object with info about each peer's state
} = state;
```

`state` 的完整的最新规范是通过 TypeScript 公开的，可以在[in the source code](https://gitlab.com/jam-systems/jam/-/blob/master/ui/jam-core/state.ts#L44)看到。

### `api`

`api` 包含各种与 Jam 交互的方法。它可以让你以编程的方式完成所有你可以在 [官方 Jam UI](https://jam.systems) 中交互完成的事情。

让我们将这些方法分为三类：

#### 1. 管理后台的房间和身份

第一类方法是 REST API 调用的包装器，用于修改 Jam 服务器上的状态。它们都是异步的，如果操作成功则返回 `true`，否则返回 `false`。

例子：

```js
const [state, api] = createJam();
let {createRoom, addSpeaker, removeSpeaker, updateIdentity} = api;

// 创建一个房间;你将是那个房间里的第一个发言者和主持人
let roomId = 'my-jam-1234';
let ok = await createRoom(roomId, {name: 'My new room'});

if (!ok) console.warn('the room possibly already existed');

console.log(state.roomId); // null, because creating a room doesn't mean being in the room

// 增加一位演讲者到房间
ok = await addSpeaker(roomId, 'kXISM4HDE4CXugmALs02mUEr4vKPK6DFJgGmhCVV7hY');

// 不把自己当成一个演讲者
ok = await removeSpeaker(roomId, state.myId);

// 更新自己的标识
ok = await updateInfo({name: 'Thomas'});

console.log(state.myIdentity.info);
// {
//   "id": "3EQBODw0NzYHTfJhsBuf1M1wggPIHbWM2mZgz2DWQpU",
//   "name": "Thomas"
// }
```

目前还没有 JavaScript 的方法可以从服务器中获取房间/身份信息，因为 Jam 会在需要的时候自动这么做，并且会通过 WebSocket 连接实时监听大部分信息。

然而，你可以使用我们的 REST API 来检查上面的调用是否成功:

```js
let roomId = 'my-jam-1234';
let response = await fetch(
  `https://jam.systems/_/pantry/api/v1/rooms/${roomId}`
);
let room = await response.json();
console.log(room);
// {
//   "name": "My new room",
//   "description": "",
//   "speakers": ["kXISM4HDE4CXugmALs02mUEr4vKPK6DFJgGmhCVV7hY"], // <-- the speaker you added
//   "moderators": ["3EQBODw0NzYHTfJhsBuf1M1wggPIHbWM2mZgz2DWQpU"], // <-- you
//   ...
// }
```

后台管理房间和身份的完整 API：

```js
let {
  // 创建房间;最初会让当前用户作为唯一的主持人和发言人
  createRoom, // async (roomId, partialRoom?: {name, ...}) => ok;

  // 更新房间;完全替换房间对象，如果没有设置主持人/发言人数组则拒绝
  updateRoom, // async (roomId, room: {name, moderators, speakers, ...}) => ok

  addSpeaker, // async (roomId, peerId) => ok
  addModerator, // async (roomId, peerId) => ok
  removeSpeaker, // async (roomId, peerId) => ok
  removeModerator, // async (roomId, peerId) => ok

  updateInfo, // async (info: {name, avatar, ...}) => ok

  addAdmin, // async (peerId) => ok, only possible for server admins
  removeAdmin, // async (peerId) => ok
} = api;
```

#### 2. 操作当前 Jam room

这些方法对应于一些用户操作。它们是异步的，并且是自动批处理的。要中断批处理并查看反映的更改，只需 `await` 返回的 Promise。

Example:

```js
const [state, api] = createJam();
let {
  setProps,
  enterRoom,
  leaveRoom,
  sendReaction,
  startRecording,
  stopRecording,
} = api;

let roomId = 'my-jam-1234';

// 设置 roomId;试着 fetch 并连接到房间
setProps({roomId});
console.log(state.roomId); // null, 因为该微任务与其他调用进行了批处理

await setProps({roomId});
console.log(state.roomId); // 'my-new-room-123', because we awaited the last call

// 在第一次用户交互时执行此操作!这让 Jam 知道我们可以安全地播放音频/使用 AudioContext
setProps({userInteracted: true});

// 加入房间
await enterRoom(roomId);
console.log(state.inRoom); // 'my-new-room-123'

// 离开房间
await leaveRoom();
console.log(state.inRoom); // null

// 再次加入房间
await enterRoom(roomId);

// 发送一些 emoji reaction
sendReaction('❤️');
sendReaction('❤️');
await sendReaction('❤️');
console.log(state.reactions);
// {
//   "3EQBODw0NzYHTfJhsBuf1M1wggPIHbWM2mZgz2DWQpU": [
//     ["❤️", 0.21084078497860237], // the numbers are just random ids
//     ["❤️", 0.7380676836914504],
//     ["❤️", 0.024757822224887427]
//   ]
// }

// the reactions will last 5 seconds
await new Promise(r => setTimeout(r, 6000));
console.log(state.reactions);
// { "3EQBODw0NzYHTfJhsBuf1M1wggPIHbWM2mZgz2DWQpU": [] }

// 录音 10 秒
await startRecording();

console.log(state.isRecording); // true

await new Promise(r => setTimeout(r, 10000));
await stopRecording();

console.log(state.isRecording); // false

await new Promise(r => setTimeout(r, 100)); // we have to wait briefly until the recording is ready
console.log(state.recordedAudio); // Blob { size: 1164, type: "audio/mp3; codecs=opus" }
```

如果你想知道什么时候执行 `setProps({roomId})`: 在官方的 Jam UI 中，我们将这个同步到当前的 URL，也就是说，它在导航到 `https://jam.systems/` 时被调用。在这种状态下，library 会急切地获取该房间，并试图与其他对等体连接，但前提是该房间存在。


操作当前 Jam room 的完整 API：

```js
let {
  setProps, // async (props) => undefined; see below

  enterRoom, // async (roomId) => undefined
  leaveRoom, // async () => undefined
  leaveStage, // async () => undefined
  sendReaction, // async (reaction) => undefined
  retryMic, // async () => undefined
  retryAudio, // async () => undefined
  autoJoinOnce, // async () => undefined

  startRecording, // async () => undefined
  stopRecording, // async () => undefined
  downloadRecording, // async (fileName) => undefined
} = api;
```

几种操作当前 Jam room 的方法组合在一个 `setProps()` 方法中，该方法接受一个应该更改的部分对象:

```js
setProps({
  roomId: 'my-jam-1234', // initial: null
  userInteracted: true, // initial: false
  micMuted: true, // initial: false,
  handRaised: true, // initial: false
});
```

#### 3. 监听变动

最后，还有一种方法可以让你监听状态的变化，例如，其他用户与房间的互动可能会触发状态变化:

```js
let {onState} = api;

// 监听指定的 `state` key
onState('peers', peers => {
  console.log('peers changed:', peers);
});

// 监听所有的 `state` 变动
onState((key, value) => {
  console.log('state change:', key, value);
});
```

`onState()` 返回一个 callback，可以用他来取消监听订阅：

```js
let {onState, setProps} = api;

let unsubscribe = onState('micMuted', muted =>
  console.log(`mic muted? ${muted}!`)
);

await setProps({micMuted: true});

// "mic muted? true!"

unsubscribe();

await setProps({micMuted: false});

// nothing logged
```

注意，在使用我们的 React 集成 [jam-core-react](https://gitlab.com/jam-systems/jam/-/tree/master/ui/packages/jam-core-react) 时，你可能不需要 `onState()`, 这将给你提供 React 钩子来订阅组件的状态更改。

### Additional API for identities

有时您可能需要手动指定用户的身份，而不是让我们创建一个随机的身份。为此，我们给你两个函数，最好在 `createJam()` 之前调用。两者都是异步:

- `importDefaultIdentity(identity)` 允许您替换当前存储在浏览器中的默认标识:

```js
import {importDefaultIdentity, createJam} from 'jam-core';

await importDefaultIdentity({
  publicKey: '...',
  secretKey: '...',
  info: {name: 'Christoph'},
});

// 输入对象上的所有属性都是可选的!

// 只需密钥就可以恢复现有用户:
await importDefaultIdentity({secretKey: '...'});

// alternatively, you can pass a seed from which the keys are created deterministically:
await importDefaultIdentity({
  seed: 'arbitrary string!',
  info: {name: 'Christoph'},
});

// 如果你既不传递密钥也不传递种子，并且一个身份已经存在，信息将会被合并到现有的一个
await importDefaultIdentity({info: {name: 'Christoph'}});

let [state, api] = createJam();

console.log(state.myIdentity.info.name); // "Christoph"
```

- `importRoomIdentity(roomId, identity)` 让你指定一个标识，它只适用于一个特定的房间:

```js
import {importRoomIdentity, createJam} from 'jam-core';

let roomId = 'the-simpsons-jam';

// API is the same as importDefaultIdentity with an additional `roomId` parameter
await importRoomIdentity(roomId, {
  secretKey: '...',
  info: {name: 'Homer Simpson'},
});
await importRoomIdentity(roomId, {secretKey: '...'});
await importRoomIdentity(roomId, {
  seed: 'arbitrary string!',
  info: {name: 'Homer Simpson'},
});
await importRoomIdentity(roomId, {info: {name: 'Homer Simpson'}});

let [state, api] = createJam();

console.log(state.myIdentity.info.name); // undefined

await setProps({roomId: 'the-simpsons-jam'});

console.log(state.myIdentity.info.name); // "Homer Simpson"
```

## Examples

带有手动 dom 更新的超级简单的自定义 UI 示例代码:https://gitlab.com/jam-systems/jam/-/blob/master/ui/examples/tiny-jam/index.html

关于在 bot 中使用 `jam-core` 的真实例子(我们将其用于 Jam 的负载测试!): https://github.com/mitschabaude/jam-bots


另外，官方的 [Jam UI](https://jam.systems) 完全建立在 `jam-core` 上(但主要通过 React 集成来访问): https://gitlab.com/jam-systems/jam/-/blob/master/ui/Jam.jsx

