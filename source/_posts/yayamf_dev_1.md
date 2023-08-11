---
title: "YAYAMF 开发记录(一)"
date: 2023-08-11
excerpt: TL;DR 浪费时间
tags:
  - YAMF
  - YAYAMF
---

这篇文章应该很乱

## YAYAMF 是什么 和 YAMF 有什么关系

YAYAMF - Yet Another YAMF 是与 YAMF 不同的另一种小窗实现

YAMF 是靠 VirtualDisplay 实现的小窗 体验上非常呃呃

YAYAMF 尝试在 window 层面实现小窗

[YAYAMF`5d91b7c`](https://github.com/duzhaokun123/YAYAMF/tree/5d91b7ccda18dcbe2a6d81fe45d45cd891c1ade9) 源码

## 所以我们做了什么吗

可以说什么都没做

不过是把系统的窗口装饰自己重写了一遍 系统自由窗口的问题一个都没解决

## 所以应该做什么

- 研究如何使 window 所在的 task 不在各种不希望被移至后台的情况下(返回主页 打开其他应用)被移至后台 (待放弃)
- 研究如何使 window 绘制在可控的 surface 上 然后通过其他能显示更高层级的方式显示

~~所以说与其干这事不如去发病~~