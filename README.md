# 🍥 Astro 主题 vhAstro-Theme

## 🚀 vhAstro-Theme：一款基于 Astro 构建的优雅的响应式博客主题

**「当极简主义遇上工程之美」**

在线演示 ➡️ [https://www.journy.online](https://www.journy.online)

官方文档 ➡️ [vhAstro-Theme](https://www.journy.online/article/astro-theme-vhastro-theme)

![Astro主题 vhAstro-Theme](https://i0.wp.com/uxiaohan.github.io/v2/2025/04/1743737394560.webp)

## ✨ 功能特性

- [x] 简洁的响应式设计
- [x] 流畅的动画和页面过渡
- [x] 丝滑的阻尼滚动效果
- [x] 顶部Banner
- [x] 两列布局
- [x] 阅读时间
- [x] 字数统计
- [x] 代码块
- [x] 语法高亮
- [x] 图片懒加载
- [x] 图片灯箱
- [x] LivePhoto
- [x] LaTex 数学公式
- [x] 赞赏功能
- [x] 评论 - 内置【Twikoo、Waline】
- [x] 本地搜索
- [x] 公告
- [x] 标签
- [x] 分类
- [x] 归档
- [x] 动态
- [x] 圈子
- [x] 关于
- [x] 留言板
- [x] 友情链接
- [x] 推荐文章
- [x] 置顶文章
- [x] 谷歌广告
- [x] 侧边栏选择性展示
- [x] 内置 404 页面
- [x] Sitemap 支持
- [x] RSS 支持
- [x] 活跃的社区支持
- [x] 广泛的现代框架兼容性
- [x] 高效的性能优化
- [x] 优秀的开发体验

## 🚀 使用方法

### 使用 Github 模板

- 使用此模板 [生成新仓库或 Fork 此仓库](https://github.com/new?template_name=vhAstro-Theme&template_owner=uxiaohan)
- 进行本地开发，Clone 新的仓库，执行 `pnpm install` 以安装依赖
- 若未安装 [pnpm](https://pnpm.io)，执行 `npm install -g pnpm`
- 通过配置文件 `src/config.ts` 自定义博客
- 执行 pnpm newpost '文章标题' 创建新文章，并在 src/content/posts/ 目录中编辑
- 参考官方指南将博客部署至 Vercel, Netlify,Cloudflare Pages, GitHub Pages 等

### Vercel 自动部署

[![vhAstro-Theme](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/uxiaohan/vhAstro-Theme)

### Cloudflare Pages 自动部署

[![vhAstro-Theme](https://deploy.workers.cloudflare.com/button)](https://dash.cloudflare.com/?to=/:account/workers-and-pages/create/deploy-to-workers&repository=https://github.com/uxiaohan/vhAstro-Theme)

### 使用命令拉取模板

```bash
# 使用 pnpm
pnpm create astro@latest --template uxiaohan/vhAstro-Theme astro-blog
# 或者 yarn
yarn create astro --template uxiaohan/vhAstro-Theme astro-blog
# 或者 npm
npm create astro@latest -- --template uxiaohan/vhAstro-Theme astro-blog
# 进入项目目录
cd astro-blog
```

### 本地开发

```bash
# 安装依赖
pnpm install
# 本地开发
pnpm dev
# 构建静态文件
pnpm build
# 创建新文章
pnpm newpost '文章标题'
```

### ⚠️ Hexo 迁移 Astro 方法

> 将 `Hexo` 博客的 `src/_posts/` 目录下的文章文件，复制到 `Astro` 的 `src/content/blog/` 目录下即可，然后自定义 `src/config.ts` 配置文件去自定义博客。<br>⚠️ `Hexo` 的部署、使用、自动化部署等方法 完全适用于 `Astro` 博客！<br>🎉 此时，你已成功迁移 Hexo 博客至 Astro 博客！

## 🍬 特色页面

### 友情链接

```js
// 配置文件 src/page_data/Link.ts
export default {
	// API 接口请求优先，数据格式保持和 data 一致
	api: "",
	// api 为空则使用 data 静态数据
	data: [
		{
			name: "韩小韩博客",
			link: "https://www.vvhan.com",
			avatar: "https://q1.qlogo.cn/g?b=qq&nk=1655466387&s=640",
			descr: "运气是计划之外的东西."
		},
		{
			name: "韩小韩API",
			link: "https://api.vvhan.com",
			avatar: "https://api.vvhan.com/static/images/logo.webp",
			descr: "免费Web API数据接口调用服务平台."
		}
	]
};
```

### 说说动态

```js
// 配置文件 src/page_data/Talking.ts
export default {
  // API 接口请求优先，数据格式保持和 data 一致
  api: '',
  // api 为空则使用 data 静态数据 
  // 注意：图片请用 vh-img-flex 类包裹
  data: [
    {
      "date": "2025-04-21 14:57:16",
      "tags": [
        "咒术回战",
      ],
      "content": "领域展开 <p class=\"vh-img-flex\"><img src=\"https://wp-cdn.4ce.cn/v2/rJ0X6Vi.png\"></p>"
    },
    {
      "date": "2025-04-20 14:57:06",
      "tags": [
        "日常"
      ],
      "content": "记录第一条说说"
    }
  ]
}
```

### 圈子（需部署 FreshRSS）

```js
// 配置文件 src/page_data/Friends.ts
export default {
  // API 接口请求优先，数据格式保持和 data 一致
  api: '',
  // api 为空则使用 data 静态数据
  data: [
    {
      "title": "Fetch的GET、POST简单HTTP请求封装",
      "auther": "韩小韩博客",
      "date": "2025-02-24",
      "link": "https://www.vvhan.com/article/fetch-get-post",
      "content": "在现代Web开发中，FetchAPI已经可以完全替代Ajax，是处理HTTP请求的利器，且支持异步操作和Promise链式调用。本文将详细介绍如何使用FetchAPI封装GET和POST请求。通过封装，代码可复用性更高，逻辑更清晰，同时还能简化错误处理和请求配置，大大提升开发效率和代码质量。GET请"
    }
  ]
}
```

## 📄 文章格式

```md
---
title: 标题
categories: 分类
tags:
  - 标签1
  - 标签2
id: 文章ID
date: 文章创建日期
updated: 文章更新日期
cover: "封面图URL (为空默认随机内置封面 /public/assets/images/banner)"
recommend: false # 是否推荐文章
top: false # 是否置顶文章
hide: false # 是否隐藏文章
<!-- 页面独有 -->
type: "links" # 页面类型
comment: false # 关闭页面评论（默认开启）
---
```

## ✅ Lighthouse

![vhAstro-Theme-Lighthouse](https://uxiaohan.github.io/v2/2025/03/1742543844078.svg)

## 🌈 项目结构

```t
.
├── public              => 静态资源
├── script              => 命令
├── src
│   ├── components      => 组件
│   ├── content
│   │   └── blog        => 博客文章数据
│   ├── layouts         => Layout 布局
│   ├── page_data       => 页面数据
│   ├── pages
│   │   ├── about                        => 关于页面
│   │   ├── archives                     => 归档页面
│   │   ├── article                      => 文章页面
│   │   ├── categories                   => 分类页面
│   │   ├── friends                      => 圈子页面
│   │   ├── links                        => 友链页面
│   │   ├── message                      => 留言页面
│   │   ├── tag                          => 标签页面
│   │   ├── talking                      => 动态页面
│   │   ├── [...page].astro              => 首页分页
│   │   ├── 404.astro                    => 404页面
│   │   ├── robots.txt.ts                => 爬虫文件
│   │   └── rss.xml.ts                   => RSS文件
│   ├── plugins             => 插件
│   ├── scripts             => 脚本
│   ├── styles              => 样式
│   ├── type                => 类型
│   ├── utils               => 工具
│   ├── content.config.ts   => 内容配置
│   ├── config.ts           => 配置
├── tsconfig.json       => Typescript 配置
├── astro.config.mjs    => Astro 配置
├── package.json        => 依赖管理
└── pnpm-lock.yaml      => 依赖锁定文件
```

## ⚙️ 项目配置
```js
export default {
  // 网站标题
  Title: 'xqq博客',
  // 网站地址
  Site: 'https://www.journy.online',
  // 网站副标题
  Subtitle: '不曾与你分享的时间,我在进步.',
  // 网站描述
  Description: 'xqq博客 一专于AI大模型算法工程师, 多精于计算机视觉、语音识别、强化学习、草莓派编程、机器人运动学等领域，致力于做出钢铁侠中的机械臂和全景语音助手',
  // 网站作者
  Author: '.Xqq',
  // 作者头像
  Avatar: 'https://wp-cdn.4ce.cn/v2/T7R5Ive.png',
  // 网站座右铭
  Motto: '运气是计划之外的东西.',
  // Cover 网站缩略图
  Cover: '/assets/images/banner/072c12ec85d2d3b5.webp',
  // 网站侧边栏公告 (不填写即不开启)
  Tips: '<p>欢迎光临我的博客 🎉</p><p>这里会分享我的日常和学习中的收集、整理及总结，希望能对你有所帮助:) 💖</p>',
  // 首页打字机文案列表
  TypeWriteList: [
    '不曾与你分享的时间,我在进步.',
    "I am making progress in the time I haven't shared with you.",
  ],
  // 网站创建时间
  CreateTime: '2025-04-20',
  // 顶部 Banner 配置
  HomeBanner: {
    enable: true,
    // 首页高度
    HomeHeight: '38.88rem',
    // 其他页面高度
    PageHeight: '28.88rem',
    // 背景
    background: "url('/assets/images/home-banner.webp') no-repeat center 60%/cover",
  },
  // 博客主题配置
  Theme: {
    // 颜色请用 16 进制颜色码
    // 主题颜色
    "--vh-main-color": "#01C4B6",
    // 字体颜色
    "--vh-font-color": "#34495e",
    // 侧边栏宽度
    "--vh-aside-width": "318px",
    // 全局圆角
    "--vh-main-radius": "0.88rem",
    // 主体内容宽度
    "--vh-main-max-width": "1458px",
  },
  // 导航栏 (新窗口打开 newWindow: true)
  Navs: [
    // 仅支持 SVG 且 SVG 需放在 public/assets/images/svg/ 目录下，填入文件名即可 <不需要文件后缀名>（封装了 SVG 组件 为了极致压缩 SVG）
    // 建议使用 https://tabler.io/icons 直接下载 SVG
    { text: '朋友', link: '/links', icon: 'Nav_friends' },
    { text: '圈子', link: '/friends', icon: 'Nav_rss' },
    { text: '动态', link: '/talking', icon: 'Nav_talking' },
    { text: '昔日', link: '/archives', icon: 'Nav_archives' },
    { text: '留言', link: '/message', icon: 'Nav_message' },
    { text: '关于', link: '/about', icon: 'Nav_about' },
    { text: 'API', link: 'https://api.vvhan.com/', target: true, icon: 'Nav_link' },
  ],
  // 侧边栏个人网站
  WebSites: [
    // 仅支持 SVG 且 SVG 需放在 public/assets/images/svg/ 目录下，填入文件名即可 <不需要文件后缀名>（封装了 SVG 组件 为了极致压缩 SVG）
    // 建议使用 https://tabler.io/icons 直接下载 SVG
    { text: 'Github', link: 'https://github.com/1onetw', icon: 'WebSite_github' },
    { text: 'API', link: 'https://api.vvhan.com', icon: 'WebSite_api' },
    { text: '塔罗牌', link: 'https://tarlo.chat', icon: 'WebSite_hot' },
    { text: 'HanAnalytics', link: 'https://analytics.vvhan.com', icon: 'WebSite_analytics' },
  ],
  // 侧边栏展示
  AsideShow: {
    // 是否展示个人网站
    WebSitesShow: true,
    // 是否展示分类
    CategoriesShow: true,
    // 是否展示标签
    TagsShow: true,
    // 是否展示推荐文章
    recommendArticleShow: true
  },
  // DNS预解析地址
  DNSOptimization: [
    'https://i0.wp.com',
    'https://cn.cravatar.com',
    'https://analytics.vvhan.com',
    'https://vh-api.4ce.cn',
    'https://registry.npmmirror.com',
    'https://pagead2.googlesyndication.com'
  ],
  // 博客音乐组件解析接口
  vhMusicApi: 'https://vh-api.4ce.cn/blog/meting',
  // 评论组件（只允许同时开启一个）
  Comment: {
    // Twikoo 评论
    Twikoo: {
      enable: false,
      envId: ''
    },
    // Waline 评论
    Waline: {
      enable: false,
      serverURL: ''
    }
  },
  // Han Analytics 统计（https://github.com/uxiaohan/HanAnalytics）
  HanAnalytics: { enable: true, server: 'https://analytics.vvhan.com', siteId: 'Hello-HanHexoBlog' },
  // Google 广告
  GoogleAds: {
    ad_Client: '', //ca-pub-xxxxxx
    // 侧边栏广告(不填不开启)
    asideAD_Slot: `<ins class="adsbygoogle" style="display:block" data-ad-client="ca-pub-xxxxxx" data-ad-slot="xxxxxx" data-ad-format="auto" data-full-width-responsive="true"></ins>`,
    // 文章页广告(不填不开启)
    articleAD_Slot: `<ins class="adsbygoogle" style="display:block" data-ad-client="ca-pub-xxxxxx" data-ad-slot="xxxxxx" data-ad-format="auto" data-full-width-responsive="true"></ins>`
  },
  // 文章内赞赏码
  Reward: {
    // 支付宝收款码
    AliPay: '/assets/images/alipay.webp',
    // 微信收款码
    WeChat: '/assets/images/wechat.webp'
  },
  // 访问网页 自动推送到搜索引擎
  SeoPush: {
    enable: false,
    serverApi: '',
    paramsName: 'url'
  },
  // 页面阻尼滚动速度
  ScrollSpeed: 666
}
```

## Twikoo 评论配置、邮箱配置

参考资料：
  1. https://twikoo.js.org/configuration.html
  2. https://twikoo.js.org/configuration.html#%E9%85%8D%E7%BD%AE

## ✨ 反馈和建议

如果您有任何建议/反馈，您可以通过我的 [电子邮件](mailto:332879882@qq.com) 联系我。或者，如果您发现错误或想要请求新功能，请随时打开问题。

## Stargazers over time

![Stargazers over time](https://starchart.cc/uxiaohan/vhAstro-Theme.svg?variant=adaptive)
