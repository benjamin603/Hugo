---
title: Hogu 部署 Twikoo 评论
description:
tags: [Hugo,Twikoo]
date: 2024-04-08T19:12:06+08:00
draft: false
isCJKLanguage: true
---

Twikoo 一个简洁、安全、免费的静态网站评论系统。

搭建过程相对比较简单，具体可参阅官方文档：**https://twikoo.js.org/**

## Hogu 部署

创建 `layouts/partials/comments.html` 文件，丢入下面代码：

```html
<div id="tcomment"></div>
<script src="https://cdn.staticfile.org/twikoo/1.6.32/twikoo.all.min.js"></script>
<script>
twikoo.init({
  envId: '您的环境id', // 腾讯云环境填 envId；Vercel 环境填地址（https://xxx.vercel.app）
  el: '#tcomment', // 容器元素
  // region: 'ap-guangzhou', // 环境地域，默认为 ap-shanghai，腾讯云环境填 ap-shanghai 或 ap-guangzhou；Vercel 环境不填
  // path: location.pathname, // 用于区分不同文章的自定义 js 路径，如果您的文章路径不是 location.pathname，需传此参数
  // lang: 'zh-CN', // 用于手动设定评论区语言，支持的语言列表 https://github.com/twikoojs/twikoo/blob/main/src/client/utils/i18n/index.js
})
</script>
```

## 更换 CDN 镜像

如果遇到默认 CDN 加载速度缓慢，可更换其他 CDN 镜像。以下为可供选择的公共 CDN，其中一些 CDN 可能需要数天时间同步最新版本：

- `https://cdn.staticfile.org/twikoo/1.6.32/twikoo.all.min.js`
- `https://lib.baomitu.com/twikoo/1.6.32/twikoo.all.min.js`
- `https://cdn.bootcdn.net/ajax/libs/twikoo/1.6.32/twikoo.all.min.js`
- `https://cdn.jsdelivr.net/npm/twikoo@1.6.32/dist/twikoo.all.min.js`

## 主题设定

在 `params.toml` 文件启用 `showComments` 即可，例：

```toml
[article]
  showComments = true
```

## 美化

### 评论框提醒文字

![评论框提醒文字](https://r2.benjamin.xin/img/posts/20240408202739.webp)

只需要添加下面css代码即可：

```css
/* Twikoo 评论提醒框 */
  /* 设置文字内容 :nth-child(1)的作用是选择第几个 */
.el-input.el-input--small.el-input-group.el-input-group--prepend:nth-child(1):before {
  content: '输入QQ号会自动获取昵称和头像🐧';
}

.el-input.el-input--small.el-input-group.el-input-group--prepend:nth-child(2):before {
  content: '收到回复将会发送到您的邮箱📧';
}

.el-input.el-input--small.el-input-group.el-input-group--prepend:nth-child(3):before {
  content: '可以通过昵称访问您的网站🔗';
}

  /* 当用户点击输入框时显示 */
.el-input.el-input--small.el-input-group.el-input-group--prepend:focus-within::before,
.el-input.el-input--small.el-input-group.el-input-group--prepend:focus-within::after {
  display: block;
}

  /* 主内容区 */
.el-input.el-input--small.el-input-group.el-input-group--prepend::before {
  /* 先隐藏起来 */
  display: none;
  /* 绝对定位 */
  position: absolute;
  /* 向上移动60像素 */
  top: -60px;
  /* 文字强制不换行，防止left:50%导致的文字换行 */
  white-space: nowrap;
  /* 圆角 */
  border-radius: 10px;
  /* 距离左边50% */
  left: 50%;
  /* 然后再向左边挪动自身的一半，即可实现居中 */
  transform: translate(-50%);
  /* 填充 */
  padding: 14px 18px;
  background: #444;
  color: #fff;
}

  /* 小角标 */
.el-input.el-input--small.el-input-group.el-input-group--prepend::after {
  display: none;
  content: '';
  position: absolute;
  /* 内容大小（宽高）为0且边框大小不为0的情况下，每一条边（4个边）都是一个三角形，组成一个正方形。
  我们先将所有边框透明，再给其中的一条边添加颜色就可以实现小三角图标 */
  border: 12px solid transparent;
  border-top-color: #444;
  left: 50%;
  transform: translate(-50%, -48px);
}
```

