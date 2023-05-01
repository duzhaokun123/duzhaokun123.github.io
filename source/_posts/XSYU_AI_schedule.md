---
title: "西安石油大学(XSYU) 教务系统课表导入小爱课程表"
date: 2022-9-27
excerpt: 已过时的
tags:
  - XSYU
  - AI_schedule
  - outdated
---

{% note info %}
已过时的 使用{% post_link new_XSYU_AI_schedule %}代替
{% endnote %}

# 0.说明

小爱课程表的导入解析源码位于 [https://github.com/duzhaokun123/XSYU_AI_schedule](https://github.com/duzhaokun123/XSYU_AI_schedule)

使用需要手动填写参数 可以在 pc 端浏览器轻松获取

# 1.操作

这里以 chrome 为例 其他浏览器差不多

## 1.1 登录教务系统

这里不多说 自己登陆

## 1.2 打开控制台

按下`F12` 没有`F12`的该考虑换个键盘了 或者你自己想办法打开

![wencnos](images/XSYU_AI_schedule/webcons.png)

## 1.3 准备抓包

在打开的 web 控制抬选择`NetWork`/`网络`选项卡

![network](images/XSYU_AI_schedule/network.png)

## 1.4 抓包

在教务系统中选择`我的课程`

加载完毕后在结果中过滤`courseTable.action`

选择负载 复制内容待用
![action](images/XSYU_AI_schedule/action.png)

## 1.5 使用

在小爱课程表中选择`直接请求接口`并按说明使用