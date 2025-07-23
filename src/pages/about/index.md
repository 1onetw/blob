---
title: "关于"
h1: "关于我"
desc: "Hi there, I’m lcz 👋"
layout: "@/layouts/PageLayout/PageLayout.astro"
type: "about"
---

:::note{type="success"}
lcz 一个记录生活、分享知识的个人博客。

我期待在这里与你分享我的见解、经验和最新的技术动态。
:::

## 联系我

```js
class LCZ {
    constructor() {
        const metaData = [
            [123, 34, 110, 97, 109, 101, 34, 58, 34, 108, 99, 122, 34, 44, 34, 101, 109, 97, 105, 108, 34, 58, 34],
            [51, 51, 50, 56, 55, 57, 56, 56, 50, 64, 113, 113, 46, 99, 111, 109, 34, 44, 34, 81, 81, 34, 58],
            [51, 51, 50, 56, 55, 57, 56, 56, 50, 44, 34, 119, 101, 99, 104, 97, 116, 34, 58, 34],
            [49, 56, 53, 51, 50, 49, 48, 52, 50, 57, 53, 34, 44, 34, 98, 105, 114, 116, 104, 34, 58],
            [50, 48, 48, 52, 44, 34, 115, 101, 120, 34, 58, 34, 30007, 34, 125]
        ];
        this.AboutLCZ = JSON.parse(String.fromCharCode(...[].concat(...metaData)));
        this.AboutLCZ.age = new Date().getFullYear() - this.AboutLCZ.birth;

        console.log("%cLCZ's Contact Protocol", "color:#00a1d6;font-size:16px;padding:4px");
        console.table(this.AboutLCZ);
    }
}

new LCZ(); 
```