---
title: "西安石油大学(XSYU) 教务系统课表导入小爱课程表"
date: 2023-02-18
excerpt: 比没有好
tags:
  - XSYU
  - AI_schedule
---

# 0.说明

小爱课程表的导入解析源码位于 [https://github.com/duzhaokun123/XSYU_AI_schedule](https://github.com/duzhaokun123/XSYU_AI_schedule)

使用需要手动提供数据

# 1.操作

就是`http://jwxt.xsyu.edu.cn/eams/courseTableForStd!courseTable.action`的响应

粘到像 https://fars.ee/ 这种能提供原始数据的网络剪贴板上

(因为那个输入框会破坏换行 我又没能力自己搓一个)

然后把能获取到数据的地址拿去解析就行