---
title: "Twikoo部署和配置"
categories: code
tags: ['Twikoo']
id: "ad3059778306af7a"
date: 2025-04-30 10:52:00
cover: "https://wp-cdn.4ce.cn/v2/M8gS8Ei.jpeg"
---

:::note
Twikoo部署和配置，给网站集成评论功能，并配置邮箱和TG通知
:::

### 为什么选择Twikoo

- 开源，免费，轻量无广告（吊打Disqus等一众评论服务）

- 匿名性好，不需要强制社交账号登录（重要‼️）

- 有新评论时可收到邮箱/即时消息通知

- 游客若留下邮箱，评论被回复时可收到邮件提醒（cusdis不支持）

- 数据支持导入导出

### 浅显理解

配置分三部分：数据库（后端），deploy平台（后端），博客网页（客户端/前端）。

这类web app的配置思路大致是：数据库负责储存数据，deploy平台来执行代码、将其变为app，最后连接到博客，从而在网页显示出来。所以必须按顺序操作，每一步都需要前一步得到的信息从而连接到一起。

### 1. 设置数据库MongoDB

[Twikoo官方文档](https://twikoo.js.org/mongodb-atlas.html)

### 2.Deploy平台：Netlify部署

原因：
Netlify对大陆访问良好，Vercel不允许大陆访问，还需自定义域名

### 3. 设置博客

需要在博客文件夹内搜索footer.html（一般主题都有这个文件，例如Astro框架）并手动加入以下代码，并将里面的您的环境id改为Netlify部署之后的URL链接'

```html
<div id="tcomment"></div>
<script src="https://cdn.staticfile.org/twikoo/1.6.16/twikoo.all.min.js"></script>
<script>
twikoo.init({
  envId: '您的环境id', // 腾讯云环境填 envId；Vercel 环境填地址（https://xxx.vercel.app）
  el: '#tcomment', // 容器元素
  // region: 'ap-guangzhou', // 环境地域，默认为 ap-shanghai，腾讯云环境填 ap-shanghai 或 ap-guangzhou；Vercel 环境不填
  // path: location.pathname, // 用于区分不同文章的自定义 js 路径，如果您的文章路径不是 location.pathname，需传此参数
  // lang: 'zh-CN', // 用于手动设定评论区语言，支持的语言列表 https://github.com/imaegoo/twikoo/blob/main/src/client/utils/i18n/index.js
})
</script>

```

### 4. Twikoo管理设置

（可选）配置Telegram机器人提醒

通过设置Telegram机器人提醒，站长能够在新评论时得到Telegram消息提示。创建telegram机器人的教程网上挺多的（比如z-lib telegram bot），以下是精简无图版本：

获取API Token：在Telegram@BotFather  的聊天界面输入 /newbot - 创建机器人名称 - 创建机器人用户名 - 得到一长串的API token

通过生成出的bot链接（格式为t.me/bot_username)进入跟bot的聊天窗口，并点击下方start激活对话

获取chat id：在@userinfobot  内输入/start- 获得的id即为自己的chat_id。把从两个bot得到的东西拼到一起，中间用 “#” 号分隔，即api_token#chat_id。示例：5262***170:AAEzkaMjOayU13fFzcg9PI7_7*****p1iAs#11111111

设置Twikoo：在Twikoo设置的“即时消息”选项下填入以下信息：
PUSHOO_CHANNEL: telegram
PUSHOO_TOKEN: api_token#chat_id
如果还是收不到消息可参考评论里@shal提供的解决方法：

@shal：给 https://t.me/LivegramBot  这个机器人发送/addbot，回复说需要发送之前创建的机器人token，然后就连接成功了。)


（可选）配置邮件通知提醒（Gmail邮箱示例）
配置邮箱通知提醒后，每当游客被博主或其他人回复时，他们就会收到邮箱提示。

参考Twikoo官方文档进行配置，可选择QQ邮箱的授权码配置