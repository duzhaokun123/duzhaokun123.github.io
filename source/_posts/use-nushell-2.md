---
title: 用更多的 Nushell
date: 2023-11-28 14:47:09
tags: [nushell]
excerpt: 在更多的环境比如 Termux
---

{% note info %}
上一篇文章 [使用 Nushell 获取好像挺跨平台的 shell 体验](http://localhost:4000/2023/11/28/use-nushell-1.html)
{% endnote %}

## 引言

用起来感觉还能用 反正好看 所以给 Termux 和 家里云(Arch Linux) 也装了

然后配置共享一下

## 安装

- [Termux](https://termux.dev/en/)

### Termux

carapace 官方源没有 从 [release](https://github.com/rsteube/carapace-bin/releases/latest) 下载

```sh
pkg install nushell starship
wget https://github.com/rsteube/carapace-bin/releases/download/v0.28.4/carapace-bin_0.28.4_linux_arm64.termux.deb
pkg install ./carapace-bin_0.28.4_linux_arm64.termux.deb
```

### 家里云(Arch Linux)

用 paru (或者其他 AUR 工具)

```bash
paru -S nushell starship carapace-bin
```

## 配置

这次直接[共享配置了](https://github.com/duzhaokun123/dot-files)

建几个软链接就好了

## 效果

![termux1](termux1.png)

各平台统一界面挺好的 :think

## 已知问题

- termux 上至少 git 补全坏的
- termux 上 sudo 缓存检查有问题 毕竟不是正经 sudo