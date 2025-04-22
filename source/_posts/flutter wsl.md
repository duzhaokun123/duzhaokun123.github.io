---
title: 在 Windows 上同时为 Windows 和 WSL 开发 Flutter 应用
date: 2025-04-09T03:27:13+08:00
excerpt: 不好用
tags: 
  - flutter
  - windows
  - wsl
---

# 在 Windows 上同时为 Windows 和 WSL 开发 Flutter 应用

{% note danger %}

这篇文章并不是一个一步到位的配环境教程

{% endnote %}

{% note info %}

如无特别说明 下文所有的 WSL 都指 WSL2

{% endnote %}

{% note info %}

作者的 Flutter SDK 安装在`E:\packages\flutter` 你可能需要根据自己的环境做调整

{% endnote %}

## 使用 本地 Flutter SDK

我们假设你在 Windows 上已经安装了 Flutter SDK, `flutter`在 PATH 里, Flutter SDK 内容未被直接修改过

对于一个典型的 Ubuntu 22.04.5 LTS WSL 环境, 我们希望通过包管理器安装 Flutter SDK

于是我们在终端输入`flutter`...

```bash
$ flutter
/usr/bin/env: ‘bash\r’: No such file or directory
```

有经验的读者可能会想到这是`LF`和`CRLF`的问题, 可以通过`dos2unix`来解决

但我们还没装 Flutter SDK 呢

这是 Windows 侧的`flutter`

```bash
$ which flutter
/mnt/e/packages/flutter/bin/flutter
```

你大概率是不想用 Windows 侧的 Flutter 的, 除非你希望每次更换平台都重装 dart sdk

```bash
$ flutter
Downloading Linux x64 Dart SDK from Flutter engine 18b71d647a292a980abb405ac7d16fe1f0b20434...
mv: cannot move '/mnt/e/packages/flutter/bin/cache/dart-sdk' to '/mnt/e/packages/flutter/bin/cache/dart-sdk.old': Permission denied
```

你大概率也不想关闭 WSL 的`appendWindowsPath`的, 除非你想手动配置 PATH

WSL 访问 Windows 路径看起来好像权限都是`rwxrwxrwx`的, 但 Windows 也是有权限管理的

我们只需要拒绝对特定文件的执行权限就好了

![权限](1.png)

于是权限变成了`-w--w--w-`, 也就是只写

那为什么不直接删了呢? 因为删了 flutter 插件就不觉得这是 Flutter SDK 了

解决了这个问题后我们通过 snap 安装了 Flutter SDK

现在我们来运行个项目测试一下

你大概是想把项目放在 Windows 侧的, 因为 Windows 侧的应用不一定支持 [UNC 路径](https://learn.microsoft.com/en-us/dotnet/standard/io/file-path-formats#unc-paths)

那么在一个在 Windows 下配置过的 Flutter 项目里`flutter run`...

![报错](2.png)

看起来好像一堆类找不到, 其实是依赖路径有问题

我们当然可以先`flutter clean`, 但是我们不想每次都先`flutter clean`

只需要在`/`里创建对应盘符的软连接就行, 比如

```bash
$ ls -l '/E:'
lrwxrwxrwx 1 root root 7 Apr  9 04:38 /E: -> /mnt/e/
```

然后它就能用了

```bash
$ flutter run
Launching lib/main.dart on Linux in debug mode...
Building Linux application...
✓ Built build/linux/x64/debug/bundle/xxx
```

Linux 下可能缺少字体, 这和本文无关

别的发行版可能构建本机应用会有问题, 这和本文无关

你可能希望 WSL 与 flutter custom device 集成, 这样就可以通过 IDE 快速运行了

在 Windows 侧

```bash
$ flutter config --enable-custom-devices
Setting "enable-custom-devices" value to "true".

You may need to restart any open editors for them to read new settings.
```

编辑`~/AppData/Roaming/.flutter_custom_devices.json`

有人可能要叫了 "你怎么在 Windows 上用 unix 风格路径名", 我乐意

```json
{
  "$schema": "file:///E:/packages/flutter/packages/flutter_tools/static/custom-devices.schema.json",
  "custom-devices": [
    {
      "id": "ubuntu",
      "label": "Ubuntu",
      "sdkNameAndVersion": "Ubuntu 22.04",
      "platform": "linux-x64",
      "enabled": true,
      "ping": [
        "true"
      ],
      "postBuild": null,
      "install": [
        "true"
      ],
      "uninstall": [
        "true"
      ],
      "runDebug": [
        "wsl",
        "-d",
        "Ubuntu-22.04",
        "/snap/bin/flutter",
        "run"
      ]
    }
  ]
}
```

```bash
$ flutter devices
Found 4 connected devices:
  Windows (desktop) • windows • windows-x64    • Microsoft Windows [Version 10.0.26100.3624]
  Chrome (web)      • chrome  • web-javascript • Google Chrome 134.0.6998.178
  Edge (web)        • edge    • web-javascript • Microsoft Edge 134.0.3124.85
  Ubuntu (mobile)   • ubuntu  • linux-x64      • Ubuntu 22.04
```

添加的设置被标注为`mobile`问题不大

但不好用 不过能用

当然 更好的方法是等待官方支持远程目标(ssh, wsl, docker)什么的