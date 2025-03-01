---
title: 假 GPU 信息提高原神特效
date: 2025-02-11 04:59:15
excerpt: 玩原神玩的
tags: 
  - android
---

原神在`Pixel Fold`上缺少一些特效 比如角色脚和地面交互 导致角色脚会穿过地面或浮在空中

![浮空](a.png)

就很糟糕

但在`OnePlus 9`上就没有这个问题

经过大量修改测试后发现与 GPU 型号有关

`Pixel Fold`的是`Mali-G710` 具体查了一下是`Mali-G710 MP7`

`OnePlus 9`的是`Adreno (TM) 660`

`GLTools`简单改一下 GPU 信息就好了

![落地](b1.png)

![落地](b2.png)

但是会图形错误比如在沙漠

但其实改成`Mail-G710 MC10`就好了..

但这么修改要从`GLTools`启动 也许以后会有用`Zygisk`的什么的

唉