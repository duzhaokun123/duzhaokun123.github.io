---
title: "简单的 LSPatch 使用教程"
date: 2022-05-06
last_update: "2022.6.1 17:58 UTC+08:00"
excerpt: 这只是一个简单的教程
tags: lspatch 
---

{% note info %}
这只是一个简单的教程
最后更新`2022.6.1 17:58 UTC+08:00`
{% endnote %}

## -1. FFAQ (Fxxking Frequently Asked Question)

### Q0: LSPatch 是什么

A0:

LSPosed框架的免 Root 实现, 通过在目标APK中插入dex等整合Xposed API

Rootless implementation of LSPosed framework, integrating Xposed API by inserting dex and so into the target APK.

来自[LSPatch 官方仓库][lspatch_repo]

### Q1: 要 Root 吗

A1: 不要

### Q2: 鸿蒙(华为)能用吗

A2: 应该可以

<!-- ### Q3: 没脑子和/或手行吗

A3: 不行 -->

## 0. 工具与环境

### 0.1 清单

- 脑子和手
- 运行 Android 9+ 的设备
- 能正常使用的 DocumentsUI(`com.google.android.documentsui`或`com.android.documentsui`) (部分魔改系统可能没有, 如 WSA) (`管理器打包`需要)
- Java 11+ (`命令行打包`需要)
- `LSPatch 管理器`和`lspatch.jar`

### 0.2 获取工具与环境

这篇文章最后更新于`2022.6.1 17:58 UTC+08:00`, 当前 [LSPatch 官方仓库][lspatch_repo]的的最新提交是[`14bc932`](https://github.com/LSPosed/LSPatch/commit/14bc932249a394000972e18ff9b03933f09d415e)

#### 0.2.1 获取 LSPatch

- 前往[LSPatch 官方仓库][lspatch_repo]的[`Actions`页面](https://github.com/LSPosed/LSPatch/actions/workflows/main.yml)下载最新构建的`Artifacts`中的`lspatch-debug`或`lspatch-release` (需要登陆 [GitHub](https://github.com))
- 前往[LSPatch 官方仓库][lspatch_repo]的[`Release`界面](https://github.com/LSPosed/LSPatch/releases/latest)下载
- 前往[@LSPatchArchives](https://t.me/LSPatchArchives)下载

#### 0.2.2 获取 Java 11+ 环境 (`命令行打包`需要)

(自己解决, 这里不是教怎么配 Java 环境的)

## 1. 管理器打包

### 1.0 准备管理器

安装下载的压缩包中的`manager-*.apk`

目前管理器还有很多功能没有实现, 但这不耽误我们打包

### 1.1 激活`Shizuku`(可选)

`Shizuku`服务用于管理器保活(目前还没实现)和自动安装, 没有问题不大

前往[`Shizuku` 用户指南](https://shizuku.rikka.app/zh-hans/guide/setup/)了解如何使用`Shizuku`

### 1.2 选择应用并打包安装

在管理器的`管理`页面点击右下角`+`

![`+`在哪里](images/simple-lspatch-guide/%2B.png)

首次打包需要选择一个目录输出打包好的文件

![选目录](images/simple-lspatch-guide/where.png)

这里随便选一个 __空目录__

选择需要 patch 的应用, 可以选择已安装的或文件, 这里以已安装的Bilibili HD 2 (com.duzhaokun123.bilibilihd2) 为例, 点击应用以开始打包

![选文件](images/simple-lspatch-guide/select.png)

之后会看到这样的界面

![准备 patch](images/simple-lspatch-guide/patch.png)

可选择`本地模式`或`便携模式`, 本地模式需要管理器保活

选择`便携模式`需要选择要嵌入模块, 点击`嵌入模块`以选择

可选多个, 选完后点击右下角对勾以继续

![选模块](images/simple-lspatch-guide/sleect2.png)

最后点击右下角`开始修补`以打包

等待完成, 点击`安装`以安装, 安装需要`Shizuku`服务, 如果没有需要去之前选择的目录手动安装

安装会卸载当前安装的应用, 注意备份

![完了](images/simple-lspatch-guide/fininh.png)

### 1.3 后续更新

更新应用需要重新打包, 如果是`便携模式`那么更新模块也需要重新打包

但管理器对已经 patch 过的应用重新打包会出错, 所以建议保留原本应用, 并通过选择文件打包

## 2. 命令行打包

### 2.0 准备文件

- `jar-*.jar`(在下载的压缩包中或其他来源中)
- 要打包的应用
- 要嵌入的模块 (可选)

### 2.1 打包

`本地模式`, 需要运行的设备安装管理器
```shell
java -jar jar-*.jar <要打包的.apk> --manager -l 2
```

`便携模式`
```shell
java -jar jar-*.jar <要打包的.apk> [-m <模块1.apk> [-m <模块2.apk> ...]] -l 2
```

### 2.2 安装

输出文件是`xx-lv2-lspatched`, 自行寻找方法安装

## 3. FAQ

### Q1 更新需要卸载吗

A1: 签名一样就不需要, 为避免签名不同可以自己签名

### Q2 管理器修补失败, 提示如图

![失败了](images/simple-lspatch-guide/fail.png)

A2: 管理器目前不能正常处理修补过的应用

重新安装回原版或选择文件再试

### Q3 `便携模式`模块没有正常加载

A4: 启用 可调试 再试

Q3.1: 还是不行

A3.1: 强行停止再试

Q3.2: 还是不行

A3.2: 重启再试

### Q4 `本地模式`模块没有正常加载

A4: 确定你选择了要加载的模块 (管理 -> 被修补应用 -> Modeule scope)

Q4.1: 还是不行

A4.1: 重启再试

### Q5 打包的应用无法分享给微信什么的, 提示签名不正确

A5: 因为签名就是和他们预期的不同, 这个无解(除非你能搞到官方签名)

### Q6 打开应用闪退, Android 8-

A6: 这个无解, 最低需要 Android 9

### Q7 打开应用闪退, Android 9+

A7: 不知道

### Q8 `本地模式`能不能选加载模块
A8: 这个功能从[`290989b`](https://github.com/LSPosed/LSPatch/commit/290989b0e6d33df85972c917819576065fac2670)开始添加 之前的版本会默认加载所有模块


[lspatch_repo]: https://github.com/LSPosed/LSPatch
