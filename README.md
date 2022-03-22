<p align="center">
  <img title='Jam mascot by @eejitlikeme'
       src="https://jam.systems/img/jam.jpg"
       width="300"
       height="300"/>
</p>

# Jam

<a href="https://discord.gg/BfakmCuXSX">![Discord](https://img.shields.io/discord/813009761468416000?label=Jam%20discord&logo=discord)</a> <a href="https://twitter.com/jam_systems">![Twitter Follow](https://img.shields.io/twitter/follow/jam_systems?style=social)</a> <a href="https://github.com/jam-systems/jam">![GitHub Repo stars](https://img.shields.io/github/stars/jam-systems/jam?style=social)</a> <a href="https://www.npmjs.com/package/jam-react">![npm](https://img.shields.io/npm/v/jam-react?label=jam-react%20on%20npm)</a>

🍓 Jam 是 Clubhouse, Twitter Spaces 等音频室的开源替代。

使用 Jam，你可以创建音频室，可以用于小组讨论、Jam 会议、自由流动的对话、辩论、戏剧、音乐剧等。唯一的限制就是你的想象力。

在 [https://jam.systems/](https://jam.systems/) 免费体验 Jam

通过我们和 **[Jam _Early Access_ Program here](https://jamshelf.com)** 托管属于你自己的 Jam 服务器。

join the [Jam community on 🎧 Discord](https://discord.gg/BfakmCuXSX)

follow [Jam on 🐣 Twitter](https://twitter.com/jam_systems)

find [Jam on 😽 Product Hunt](https://www.producthunt.com/posts/jam-d17ff3cc-556c-4c17-8140-5211cb1cd81f)

🗓 join our weekly Jam Jam (a Jam where we jam about Jam) every [Wed @ 8pm CEST (2pm EDT)](http://jam.systems/jam-jam-ns4a)


add the `/jam` shortcut to your Slack workspace:

<a href="https://slack.com/oauth/v2/authorize?client_id=1827991458162.1827997742338&scope=chat:write,chat:write.public,commands&user_scope="><img alt="Add to Slack" height="40" width="139" src="https://platform.slack-edge.com/img/add_to_slack@2x.png" /></a>

add Jam to your [Wordpress](https://medium.com/jam/add-jam-to-wordpress-ca1932cd8ff3), [Webflow](https://medium.com/jam/add-jam-to-webflow-d8a680959007) or [Shopify](https://medium.com/jam/add-jam-to-shopify-a77865cd7b6f):

<a href="https://medium.com/jam/add-jam-to-wordpress-ca1932cd8ff3"><img alt="Add Jam to Wordpress" src="https://s.w.org/style/images/about/WordPress-logotype-standard.png" height="40" /></a> &nbsp;&nbsp;&nbsp; <a href="https://medium.com/jam/add-jam-to-webflow-d8a680959007"><img alt="Add Jam to Webflow" src="https://upload.wikimedia.org/wikipedia/commons/9/92/Webflow_logo.svg" height="22" /></a> &nbsp;&nbsp;&nbsp; <a href="https://medium.com/jam/add-jam-to-shopify-a77865cd7b6f"><img alt="Add Jam to Shopify" src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/0e/Shopify_logo_2018.svg/430px-Shopify_logo_2018.svg.png" height="22" /></a>

## 特性对比

🍓 Jam 与 Clubhouse 或 Twitter Spaces 等类似服务的比较概述。随着服务的发展，我们打算让这个表保持最新。请让我们知道您关心什么，以及我们如何让您更容易地了解 🍓 Jam 是否适合您。

|                                 | 🍓 Jam                 | 👋 Clubhouse      | 🐣 Twitter Spaces          |
|---------------------------------|------------------------|--------------------|----------------------------|
| 🎨 Branded Rooms                | ✅ 设置你自己的 logo 和颜色   | ❌                | ❌                  |
| 🧩 Embed in your app or website | ✅ iFrame, WebView, JS SDK    | ❌                | ❌                  |
| 💯 Animated Reactions           | ✅ (❤️ 💯 😂 😅 😳 🤔)| ❌                | ✅ (💯 ✋ ✊ ✌️ 👋)       |
| 📎 Description w/ Links         | ✅ w/ Markdown support | ❌                | ☑️ show tweets              |
| 💵 Earn Money                   | ✅ dedicated support for<br/>PayPal.me, Bitcoin:, Gumroad | ❌ no built in support<br/><br/>currently not allowed<br/>unless specifically authorized<br/> see Terms of Service<br/>[(Conditions of Use #9)](https://www.notion.so/Terms-of-Service-cfbd1824d4704e1fa4a83f0312b8cf88) | ❌  no built in support |
| 📱 Platform support             | ✅ Web, iOS, Android,<br/>macOS, Windows, Linux, … | ☑️ iOS App Store & invite | ☑️ iOS App Store & invite |
| 🌏 [Data Sovereignty](https://en.wikipedia.org/wiki/Data_sovereignty) | ☑️ EU 🇪🇺 (Frankfurt, Germany 🇩🇪)<br/><br/> ✅ open source, host wherever you want | ☑️ USA 🇺🇸, China 🇨🇳 (Audio API Provider)<br/><br/>❌ proprietary, no self-hosting   | ☑️ USA 🇺🇸 (?)<br/><br/>❌ proprietary, no self-hosting |
| 🎙 Number of Speakers in a Room | ✅ ~ 15+ (limited by p2p bandwidth) | ✅ ~ 100s (?) | ☑️ 10             |
| 🎫 Number of People in a Room   | ✅ unlimited (depending on server)  | ✅ up to 8000 (?)     | ✅ unlimited (?) |
| 🗓 Discovery                    | ☑️ off-platform (social networks, messengers) | ✅ hallway, upcoming events, off-platform | ✅ fleet bar, off-platform |

💯 Animated Reactions (🎬 [see a demo](https://twitter.com/__tosh/status/1362558104412565504/video/1))

🎨 Branded Rooms: 为你房间设置图片和颜色

💵 Earn Money: dedicated support for linkinging to off-platform services to charge money or sell your own products

## Room Configuration via URL

您可以通过添加参数作为查询参数或在 url 的哈希组件中作为 [`base64URL`](https://en.wikipedia.org/wiki/Base64#The_URL_applications) 编码字符串来配置一个房间。

你可以在我们的 [examples](./examples) 目录或 [Glide 教程](https://medium.com/jam/lets-build-a-micro-clubhouse-using-glide-and-jam-32597368fc98) 中通过 url 查看房间配置的例子。

| key                  | value                     | note |
|----------------------|---------------------------|------|
| `room.name`          | 房间名          |      |
| `room.description`   | 房间描述   |      |
| `room.color`         | CSS 值 (e.g. "red" #CCCCCC) | 房间的主颜色，用于主操作按钮和一些样式，如背景铬色，注意，如果作为查询参数传递，您需要使用%23编码哈希符(#CCCCCC becomes %23CCCCCC) |
| `room.stageOnly`     | true                      | users join directly on stage instead of in the audience |
| `ux.noLeave`         | true                      | 移除 "leave" 按钮 |
| `ux.autoCreate`      | true                      | 如果不存在那么就创建房间 |
| `ux.autoJoin`        | true                      | 用户自动加入房间，不需要点击加入按钮|
| `ux.autoRejoin`      | true                      | 如果用户以前在这个房间里，他们不需要点击按钮就可以重新加入这个房间 |
| `identity.name`      | 用户名name of the user          |      |
| `identity.avatar`    | 用户头像 URL     | avatar, profile picture, user photo … |
| `keys.seed`    | string seed for deriving a public/private keypair     | a seed for deriving a public/private keypair. this keypair is for the current user within the room (related: creating rooms with a known set of moderators requires the public keys of the moderators) |

## Known Issues and Solutions

**📱 iPhone: audio output sometimes switches randomly between loudspeaker and earspeaker.**

Workaround: use bluetooth or cable headphones, this way audio will always go through the headphones.

**📱 iPhone & Android: when phone goes to sleep/lockscreen because of inactivity the microphone or sound output might stop working until you unlock the screen again**

Workaround: make sure you are using Jam in the standalone browser instead of within a webview

**Participants can hear me but I can not hear them**

When participants join a room on 🍓 Jam they start in the _audience_, you can add them to the stage by tapping on them and by tapping the `"invite to stage"` button

## Host Your Own Server

Hosting your own Jam server is easy.

### Minimum Requirements

要运行您自己的 Jam 实例，我们建议至少使用 1GB RAM 和 1GHz CPU。

e.g.: a Raspberry Pi (1+ GB RAM) or the smallest [Digital Ocean Basic Droplet (1 GB RAM)](https://www.digitalocean.com/pricing/) or the smallest [Linode shared plan (1 GB RAM)](https://www.linode.com/pricing/) or [t2.micro (1 GB RAM) on Amazon Web Services](https://aws.amazon.com/ec2/instance-types/t2/) should be enough to get started.

### Install

1. Install docker and docker-compose (https://docs.docker.com/compose/install/)
1. `git clone https://gitlab.com/jam-systems/jam.git`
1. `cd jam`
1. `git checkout stable`  
1. `cd deployment`
1. `cp .env.example .env`
1. `nano .env` set `JAM_HOST` to the domain you want Jam to be available (If you are deploying on AWS you need a domain you own pointing to your sever as letsencrypt does not issue certificates for `*.compute.amazonaws.com` domains)
1. In your DNS settings point `${JAM_HOST}`, and `*.${JAM_HOST}` to your IP address (if you don't want a wildcard you need the subdomains `stun` and `turn` (e.g. stun.jam.example.com and turn.jam.example.com))
1. If you are behind a NAT:
   1. Open ports 3478 and 3480, both TCP and UDP, and 80 and 443, TCP, on your firewall
   1. `nano turnserver.conf` set `realm` to your domain. If you are running coturn behind NAT, you may need to add the parameter  `external-ip` and give it the value of your public IP address.
1. `docker-compose up -d`

### Update

**NOTE:** Make sure you have the newest version of docker-compose (install according to https://docs.docker.com/compose/install/).

1. `cd jam/deployment`
2. `git checkout stable`
3. `git pull`
4. `docker-compose pull`
5. `docker-compose up -d`

**NOTE:** If you update from a version before March 23rd to one after and you want to keep users' identities and rooms:

1. `cd jam/deployment`
2. `docker-compose down`   
3. `git checkout stable`
4. `git pull`
5. `docker-compose pull`
6. `mv ../pantryredis ../data`   
7. `docker-compose up -d`


## Jam SDK

如果你想从头开始构建自己的音频室 UI，或者甚至开发一个 Jam bot，那么 Jam 也会提供给你一些工具来完成这些工作！看看 [jam-core](https://gitlab.com/jam-systems/jam/-/tree/master/ui/packages/jam-core), 我们的 npm 包，它将 Jam 的所有功能公开为一个 JavaScript 库，而没有规定任何 UI。

要想轻松地将 `jam-core` 集成到 React 应用中，请查看配套包 [jam-core-react](https://gitlab.com/jam-systems/jam/-/tree/master/ui/packages/jam-core-react)。官方 Jam 应用本身主要是建立在 `jam-core-react` 的基础上。

## Develop

在 `ui` 目录中，使用 `yarn` 来安装依赖项，使用 `yarn start` 来启动本地开发服务器。

目录概述：

`deployment`/ 部署和托管 Jam 的 docker compose 文件

`pantry`/ 用于处理 Jam 的身份验证和协调的轻量级服务器

`ui`/ 基于 React 框架的 web 用户界面

## Buy Us ☕

**BTC:** 3HM1zPtLuwCGarbihNYVjFVwbFrFe9keqh

**ETH:** 0xe15265b2a309f0d20038e10b8df5a12fb5e916f8
