---
title: Windows 11 上的 Win32(UxTheme) 应用全局暗色
date: 2025-03
excerpt: 使用高对比度主题 (划掉)
tags: windows
---

{% note danger %}

警告

修改系统文件可能会导致系统无法正常工作 操作前做好备份

{% endnote %}
    

Win32(UxTheme) 应用在 Windows 上就什么官方暗色支持

比如微软这篇文档 [Support Dark and Light themes in Win32 apps (Archive)](https://web.archive.org/web/20250221212657/https://learn.microsoft.com/en-us/windows/apps/desktop/modernize/ui/apply-windows-themes) 连一个判断是否暗色主题的 API 都没有

但也不是完全没有 比如典型的文件选择器就有暗色支持

我们打开 msstyle 文件看看 就有很多带`DarkMode`的资源

![](msstyle1.png)

也确实有 Win32 应用使用暗色主题的 undocumented 的方法

- [Win32 Dark Mode](https://gist.github.com/rounk-ctrl/b04e5622e30e0d62956870d5c22b7017)
- [win32-darkmode](https://github.com/ysc3839/win32-darkmode)

但这些都靠的是开发者使劲的 那我们能不能自己使劲呢

当然可以 也就几千个值没文档效果全靠猜 你看这不才 9003 个值 还没破万

![9003](large.png)

其实可以把`DarkMode`的资源复制到普通资源上 没有`DarkMode`的再说

那它就像这个样子

![](cp1.png)

也就是说剩下的就要自己修了