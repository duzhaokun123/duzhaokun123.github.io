---
title: "LSPatch 处理分包应用"
date: 2022-08-09
last_update: "2022.8.9 21:30 UTC+08:00"
excerpt: 这可能没那么简单 但他还是很简单
tags:
 - lspatch
---

最后更新`2022.8.9 21:30 UTC+08:00`

这篇文章假设你已经看了{% post_link simple-lspatch-guide %}

# 1. 解压文件

apks mapk xapk 或者别的什么分包格式 到随便什么路径

这里以 微信 play 版 的 xapk 为例

其他分包格式可能有不同布局 但总体上差不多

![WeChat xapk](images/lspatch-apks/wechat.jpg)

# 2. 打包

这里使用 lspatch 应用

选择应用时选`从存储目录选择(多个)apk`

在刚才解压的目录选择文件 __长按__ 以选择 __多个__ 文件

选择需要的文件 如果不知道选什么 选择 __所有__ `apk`文件

<details><summary>长图警告</summary>

<img src="/images/lspatch-apks/select.jpg">

</details>

按右上角`选择`完成选择 可能会卡一下

之后按正常流程打包

# 3. 安装

建议激活`Shizuku`后通过应用直接安装

否则 生成的文件在之前选择的路径 有多个 自己想办法安装

注意: 只安装主 apk 大多数情况下会无法打开应用 这不是 lspatch 的问题

# 4. FAQ

## 4.1 选择完应用后报错

![错](images/lspatch-apks/err.png)

选择了不来自同一分包文件的`apk`