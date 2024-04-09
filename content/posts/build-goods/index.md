---
title: Hogu 部署好物页面
description:
tags: [Hugo,好物]
date: 2023-05-01T11:12:06+08:00
draft: false
isCJKLanguage: true
---

之前使用 `Hexo` 博客时候搭建过一个**好物推荐**的页面，迁到 Hexo 后搜了以下，[林木木](https://immmmm.com/) 童鞋出过一期教程，因为时间久远，部分资源已经无法获取，所以本文重新记录下搭建的过程！

![好物推荐](https://r2.benjamin.xin/img/posts/346shots_so.webp)

## 实现思路

利用 `getJSON` 函数统一维护页面数据，后期更新修改在 `data/goods.json` 中，不再需要动页面模板。

## 部署过程

### 数据

创建 `data\goods.json` 文件，并填入下方代码：

```json
{
    "good": [
        {
            "image": "/goods/cuktech-1W.webp",
            "jiage": "¥99 (23-12)",
            "title": "酷态科口袋充电宝",
            "link": "",
            "note": "名片大小，小巧玲珑，颜值在线，支持 30W 快充。"
        },{
            "image": "/goods/mijia-cqb2.webp",
            "jiage": "¥168 (23-10)",
            "title": "米家充气宝2",
            "link": "",
            "note": "充气虽然慢了点，发热有点厉害，但值得常备。"
        },{
            "image": "/goods/vaydeer.webp",
            "jiage": "¥204 (23-10)",
            "title": "Mac mini 立式扩展",
            "link": "",
            "note": "虽塑料材质，但接口全也满速，还可内置 ssd。立起来后，桌面整洁不少。"
        },{
            "image": "/goods/appleTV.webp",
            "jiage": "¥1177 (23-06)",
            "title": "Apple TV 7代",
            "link": "https://npcitem.jd.hk/100045175467.html",
            "note": "看到最新系统已支持网络设置，省去一个软路由，下单之！"
        },
        {
            "image": "/goods/whirlpool.webp",
            "jiage": "¥1856 (23-01)",
            "title": "Whirlpool 净水器",
            "link": "https://item.jd.com/10030311404093.html",
            "note": "自用还是舒适的，最高温度90°+，泡泡挂耳咖啡也是足够的。奇怪的是这款停产了？"
        },
        {
            "image": "/goods/yeelight.webp",
            "jiage": "¥589 (23-01)",
            "title": "Yeelight 屏幕挂灯 Pro",
            "link": "https://cn.yeelight.com/zh_CN/consumer-grade",
            "note": "早买早享受系列，Pro 多了一个背光氛围灯。"
        }
    ]
}
```

### 模板

创建 `layouts\_dafault\goods.html` 文件并填入下面代码：

```html
{{ define "main" }}
{{ $goods := getJSON "data/goods.json" }}

<div class="post">
  <div class="post-content goods">
  {{ .Content }}
  <div id="goods">
  {{ range $goods.good}}
  <div class="goods-bankuai img-hide">
    <div class="goods-duiqi"><img loading="lazy" decoding="async" src="{{ .image }}"></div>
    <div class="goods-jiage">{{ .jiage }}</div>
    <div class="goods-title"><a href="{{ .link }}">{{ .title }}</a></div>
    <div class="goods-note">{{ .note }}</div>
  </div>
  {{ end }}
  </div>
  </div>

</div>
{{ end }}
```

### 页面

新建 `content\goods\_index.md` ，加入一些内容（也可以使用空白内容）

### 样式

自定义 `CSS` 样式：

```css
/* 好物页面 */
:root {
  --code-bg: #fafafa
}

.dark {
  --code-bg: #3b3d42
}

div#goods {
    padding-top: 1rem;
}

.goods-bankuai {
  display: inline-block;
  vertical-align: top;
  width: calc(33.33% - 16px);
  height: 326px;
  margin: 0 8px 12px 0;
  list-style: none;
  border-radius: 8px;
  background: var(--code-bg);
  padding: 0 16px;
  overflow: hidden;
}

.goods-duiqi {
  min-height: 164px;
  display: flex;
  justify-content: center;
  align-items: center
}

.goods-duiqi img {
  width: 80%;
  margin: 0;
  transition: transform .2s ease-in-out;
  cursor: pointer
}

.goods-duiqi:hover img {
  transform: translateY(-4px)
}

.goods-title {
  font-size: 14px;
  margin: 0;
  transition: .5s
}

.goods-title a {
  font-size: 14px;
  text-decoration: none
}

.goods-jiage {
  color: #999;
  font-size: 14px;
  line-height: 1.4rem
}

.goods-note {
  color: #999;
  font-size: 14px;
  line-height: 1.3rem
}

@media (max-width:700px) {
  .goods-bankuai {
    width: 100%;
    height: 100%;
    padding: 0 2% 5% 2%
  }

  .goods-title {
    font-size: 14px;
    margin: 0 10px!important;
    transition: .5s
  }

  .goods-jiage {
    margin: 0 10px 0 10px
  }

  .goods-duiqi img {
    width: 50%;
    margin: 0
  }

  .goods-note {
    line-height: 1.3rem;
    margin: 8px 10px 0 10px
  }

  .goods-title {
    font-size: 14px;
    margin: 0 auto;
    line-height: 1.5rem
  }

  .goods-title a {
    font-size: 14px;
    text-decoration: none;
    box-shadow: none
  }
}

@media screen and (min-width:700px) and (max-width:900px) {
  .goods-quanju {
    font-size: 0;
    width: 106%
  }

  .goods-bankuai {
    display: inline-block;
    vertical-align: top;
    width: 40%;
    margin-right: 15px;
    height: 420px;
  }

  .goods-title {
    font-size: 14px;
    margin: 0 auto;
    line-height: 1.5rem;
    transition: .5s
  }

  .goods-title a {
    font-size: 14px;
    text-decoration: none;
    box-shadow: none
  }
}

@media (min-width:900px) {
  .goods-quanju {
    font-size: 0;
    width: 106%
  }

  .goods-bankuai {
    display: inline-block;
    vertical-align: top;
    width: calc(33.33% - 16px);
    height: 390px;
    margin: 0 8px 12px 0
  }

  .goods-note {
    font-size: 14px;
    line-height: 1.3rem;
    margin-top: .5rem
  }

  .goods-duiqi img {
    width: 80%;
    margin: 0
  }

  .goods-bankuai.img-hide img {
    transition: transform .2s ease-in-out;
    cursor: pointer
  }

  .goods-bankuai.img-hide:hover img {
    transform: translateY(-4px)
  }

  .goods-title {
    font-size: 16px;
    margin: 0 auto;
    line-height: 1.6rem;
    transition: .5s
  }

  .goods-title a {
    font-size: 16px;
    text-decoration: none;
    box-shadow: none;
    transition: .5s
  }
}
```

## 结语

搭建完成后我们只需要在 `goods.json` 文件更新数据即可，`CSS` 样式文件根据自己的主题做调整！