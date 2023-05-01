---
title: "EhViewer: xiaojieonly VS. NekoInverter"
date: 2021-11-17
excerpt: 已过时 已实际体验为准
tags:
  - ehviewer
  - outdated
---

{% note danger %}
已过时 已实际体验为准
{% endnote %}

{% note warning %}
这篇文章主要关于本人对这两个版本的看法, 不保证客观公正, 但内容皆为事实
{% endnote %}

## 引言
这里就不介绍 EhViewer 了, 想了解看 [https://nicebowl.moe/11](https://nicebowl.moe/11)

这里主要将 Hippo 不再公开更新后, 两个用户较为广泛的发行版做对比
- xiaojieonly [https://github.com/xiaojieonly/Ehviewer_CN_SXJ](https://github.com/xiaojieonly/Ehviewer_CN_SXJ) 以下简称`xiaojie`
- NekoInverter [https://gitlab.com/NekoInverter/EhViewer](https://gitlab.com/NekoInverter/EhViewer) 以下简称`neko`

截止本文章发布(2021.11.17), `xiaojie`最新版为`1.8.8.0(109)`, `neko`最新版为`1.7.24.3(1000000)`

图有些小, 可以在新标签页打开查看

以下皆为最新版的对比

## 对比
### 应用本身
#### 界面
个人认为`neko`界面更好看, 对`edge-to-edge`有处理

|`xiaojie`|`neko`|
|:-:|:-:|
|![xiaojie_home](images/ehv-xiaojie-vs-neko/xiaojie_home.png)|![neko_home](images/ehv-xiaojie-vs-neko/neko_home.png)|
|![xiaojie_settings](images/ehv-xiaojie-vs-neko/xiaojie_settings.png)|![neko_settings](images/ehv-xiaojie-vs-neko/neko_settings.png)|

#### 原生库
`neko`上了64位, `xiaojie`认为没必要 [`xiaojie`#140](https://github.com/xiaojieonly/Ehviewer_CN_SXJ/issues/140)

|`xiaojie`|`neko`|
|:-:|:-:|
|![xiaojie_native](images/ehv-xiaojie-vs-neko/xiaojie_native.png)|![neko_native](images/ehv-xiaojie-vs-neko/neko_native.png)|

#### 权限
`xiaojie`还没用[SAF](https://developer.android.com/guide/topics/providers/document-provider?hl=zh-cn), 要请求`WRITE_EXTERNAL_STORAGE`

|`xiaojie`|`neko`|
|:-:|:-:|
|![xiaojie_access](images/ehv-xiaojie-vs-neko/xiaojie_access.png)|![neko_access](images/ehv-xiaojie-vs-neko/neko_access.png)|

#### bug
- `xiaojie`的`域名前置`可能导致和新版本 Clash for Android 冲突导致网络错误的问题 [`xiaojie`#116](https://github.com/xiaojieonly/Ehviewer_CN_SXJ/issues/166), `neko`已修复(1.7.23.3 (148))
- `xiaojie`的备份文件与其他客户端不兼容 [`xiaojie`#168](https://github.com/xiaojieonly/Ehviewer_CN_SXJ/issues/168)

### 作者
`xiaojie`主要在 GitHub 活动, 不熟

`neko`在网上能查到`neko`的黑历史, 且本人被拉黑过, 但本人不主张因人废言

### 社区
`xiaojie`的 issues 界面可以说非常的乱, `xiaojie`有 [telegram 用户组](https://t.me/Ehviewer_xiaojieonly_channel)

`neko`在 GitLab 在国内可能太小众了, 不是很乱, 没有官方用户组

### 开源
`xiaojie`[并没有完全开源](https://github.com/xiaojieonly/Ehviewer_CN_SXJ#%E8%AF%B7%E4%BB%94%E7%BB%86%E4%BA%86%E8%A7%A3%E4%B8%AA%E7%89%88%E6%9C%AC%E6%9B%B4%E6%96%B0%E6%97%A5%E5%BF%97%E4%BB%A5%E9%98%B2%E6%AD%A2%E8%B7%B3%E8%BF%87%E6%9F%90%E4%BA%9B%E9%87%8D%E8%A6%81%E7%89%88%E6%9C%AC%E5%AF%BC%E8%87%B4%E6%97%A0%E6%B3%95%E4%BD%BF%E7%94%A8app%E5%87%BA%E4%BA%8E%E5%AF%B9%E5%88%86%E4%BA%AB%E6%95%B0%E6%8D%AE%E7%9A%84%E5%AE%89%E5%85%A8%E6%80%A7%E7%9A%84%E8%80%83%E8%99%91%E7%9B%AE%E5%89%8D%E6%9A%82%E4%B8%8D%E6%8F%90%E4%BA%A4%E5%88%86%E4%BA%AB%E5%8A%9F%E8%83%BD%E4%B8%AD%E7%9A%84%E5%8A%A0%E5%AF%86%E4%BB%A3%E7%A0%81%E7%AD%89%E4%B9%8B%E5%90%8E%E5%8A%9F%E8%83%BD%E5%AE%8C%E5%85%A8%E6%88%90%E7%86%9F%E5%90%8E%E5%86%8D%E6%8F%90%E4%BA%A4)

`neko`完全开源

## 总结
建于以上的对比, 本人选择了`neko`, 主要是因为界面好看
