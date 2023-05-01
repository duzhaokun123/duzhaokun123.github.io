---
title: "VirtualDisplay 实现的小窗 与 toast 窗口 与 systemui 崩溃"
date: 2023-02-23
excerpt: TL;DR 加上`VIRTUAL_DISPLAY_FLAG_PUBLIC`
tags:
  - YAMF
  - VirtualDisplay
---

# 问题

有人给[米窗](https://github.com/sunshine0523/Mi-FreeForm)反馈了这样的问题

[在特定的情况下使用米窗会导致 systemui 崩溃](https://github.com/sunshine0523/Mi-FreeForm/issues/20)

具体的崩溃是这样的

```log
02-23 14:44:45.581  6203  6203 E AndroidRuntime: FATAL EXCEPTION: main
02-23 14:44:45.581  6203  6203 E AndroidRuntime: Process: com.android.systemui, PID: 6203
02-23 14:44:45.581  6203  6203 E AndroidRuntime: java.lang.IllegalArgumentException: display must not be null
02-23 14:44:45.581  6203  6203 E AndroidRuntime: 	at android.app.ContextImpl.createDisplayContext(ContextImpl.java:2664)
02-23 14:44:45.581  6203  6203 E AndroidRuntime: 	at com.android.systemui.toast.ToastUI$$ExternalSyntheticLambda0.run(R8$$SyntheticClass:48)
02-23 14:44:45.581  6203  6203 E AndroidRuntime: 	at com.android.systemui.toast.ToastUI.showToast(ToastUI.java:36)
02-23 14:44:45.581  6203  6203 E AndroidRuntime: 	at com.android.systemui.statusbar.CommandQueue$H.handleMessage(CommandQueue.java:712)
02-23 14:44:45.581  6203  6203 E AndroidRuntime: 	at android.os.Handler.dispatchMessage(Handler.java:106)
02-23 14:44:45.581  6203  6203 E AndroidRuntime: 	at android.os.Looper.loopOnce(Looper.java:201)
02-23 14:44:45.581  6203  6203 E AndroidRuntime: 	at android.os.Looper.loop(Looper.java:288)
02-23 14:44:45.581  6203  6203 E AndroidRuntime: 	at android.app.ActivityThread.main(ActivityThread.java:7872)
02-23 14:44:45.581  6203  6203 E AndroidRuntime: 	at java.lang.reflect.Method.invoke(Native Method)
02-23 14:44:45.581  6203  6203 E AndroidRuntime: 	at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:548)
02-23 14:44:45.581  6203  6203 E AndroidRuntime: 	at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:946)
```

看起来像有 toast 试图在不存在的 display 上显示导致系统崩溃

# 分析

就我个人来说 虽然认为 toast 可以在非主屏幕上显示 应该是用 displayid 变量控制的

但从来没实践过 因为平时都是用 application 直接创建 toast 的(因为方便 很多框架都是这样操作的 所以几乎没有人反馈这样的问题)

借此机会 对比了一下使用 activity 和 application 创建 toast 的区别 确实在 [YAMF](https://github.com/duzhaokun123/YAMF) 上以相同的原因导致了 systemui 崩溃

但好在 YAMF 可以方便地修改 flags 加上`VIRTUAL_DISPLAY_FLAG_PUBLIC`后 toast 正常显示

所以导致这一问题的一个原因是 toast 试图在非主屏幕上显示

通过阅读代码 容易发现

显示 toast 的过程大概是这样的
```txt
Toast.show() -> NotificationManagerService.enqueueToast() -> ToastUI.showToast()
```

## 什么决定了 toast 显示的 display

简单阅读代码 很容易发现 在`android.widget.Toast.show()`中

```java
public void show() {
    // ...
    final int displayId = mContext.getDisplayId();
    // ...
}
```

displayId 就是使用的 context 的 displayId

那么 context 的 displayid 是怎么来的

再次阅读代码 发现在`android.app.ContextImpl`中

```java
public Display getDisplayNoVerify() {
    if (mDisplay == null) {
        return mResourcesManager.getAdjustedDisplay(Display.DEFAULT_DISPLAY,mResources);
    }

    return mDisplay;
}

public int getDisplayId() {
    final Display display = getDisplayNoVerify();
    return display != null ? display.getDisplayId() : Display.DEFAULT_DISPLAY;
}
```

如果`mDisplay`为`null` 则返回默认 display 其 id 为 0
否则返回所在的 display 的 id

在运行时通过反射很容易发现 application 的`mDisplay`为`null`而 activity 的为所在 display

## 什么导致了找不到 display

通过上面的测试很容易发现 没有设置`VIRTUAL_DISPLAY_FLAG_PUBLIC`的 virtualDisplay 会找不到

具体为什么还没看

# 解决

两个方法
- 加上`VIRTUAL_DISPLAY_FLAG_PUBLIC`
  - 相对容易 不需要 hook 系统 (但会导致一些 ui 上的变化)
- 修改 toast 的 displayid 到 0
  - 相对麻烦 需要 hook 系统(而且 toast 在错误的屏幕上显示很糟糕 (虽然很少有应用会用 activity 创建 toast 所以其实很多都会在错误的屏幕上显示的))
