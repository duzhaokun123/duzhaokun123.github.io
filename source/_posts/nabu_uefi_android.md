---
title: Xiaomi Pad 5 (nabu) 从 UEFI 启动 Android
date: 2025-04-23T05:22:52+08:00
excerpt: 鉴定为 吃饱了撑的
tags:
  - android
  - nabu
---

# Xiaomi Pad 5 (nabu) 从 UEFI 启动 Android

{% note info %}

虽然目标设备是 nabu, 但我们尽可能把它写得通用

{% endnote %}

## 已知问题

- 显示闪烁
- 触摸不工作
- 设备加密不工作 (QSEE 不工作)

这个闪烁很严重 我不清楚是否会对设备硬件造成损害 但一定会对我的硬件造成损害

## 准备

### 安装需要

- 设备
- 设备的 UEFI 环境 (比如 [Project Aloha](https://github.com/Project-Aloha/mu_aloha_platforms))
- 设备的 Android 系统 (我们不打算搞通用系统)
- simple init (https://github.com/BigfootACA/simple-init)

### 调试额外需要

- 设备的 Android 系统的内核源码 (能跑的)

## 配置 esp 目录

大概这样
```tree
EFI/
├── android/
│   ├── Image
│   ├── nabu-sm8150-overlay.dtbo
│   ├── sm8150-v2-xiaomi.dtb
│   └── ramdisk.cpio.xz
└── BOOT/
    └── BOOTAA64.efi
```

## 获取内核文件

通常在 boot 里, 提取出来解压到未压缩

## 获取 ramdisk

其实是 initrd 更其实是 initramfs

对于 nabu

boot 里有通用 ramdisk

vendor_boot 里有 vendor ramdisk

提取出来解压到只剩 cpio

## 获取 dtb

对于 nabu

vendor_boot 里有 dtb

dtbo 里有 dtbo

注意内核参数`androidboot.dtb_idx=n`和`androidboot.dtbo_idx=n`指定使用的 dtb 和 dtbo 的索引

当然也可以从内核源码编译或从`/proc/device-tree`提取

## 获取内核参数

就是`/proc/cmdline`的内容

## 配置 simple init

simple init 不支持多 initrd, 所以我们得合并一下

vendor 在前 通用在后

配置一个 Linux 引导项 注意 uefi = false

即使你的内核配置了`CONFIG_EFI_STUB`, 也要设置为 false, 我们还没有对 UEFI 环境做好准备

simple init 的文档很绕, 但这里有个简单的能用的配置供参考

```properties
language = "zh_CN"

boot.configs.aaa.mode = 8
boot.configs.aaa.desc = "Android legacy"
boot.configs.aaa.show = true
boot.configs.aaa.enabled = true
boot.configs.aaa.icon = "android.svg"
boot.configs.aaa.extra.kernel = "@part_esp:\\EFI\\android\\Image"
boot.configs.aaa.extra.initrd = "@part_esp:\\EFI\\android\\ramdisk.cpio.xz"
boot.configs.aaa.extra.dtb = "@part_esp:\\EFI\\android\\sm8150-v2-xiaomi.dtb"
boot.configs.aaa.extra.dtbo = "@part_esp:\\EFI\\android\\nabu-sm8150-overlay.dtbo"
boot.configs.aaa.extra.cmdline = "androidboot.force_normal_boot=1 androidboot.hardware=qcom androidboot.memcg=1 androidboot.usbcontroller=a600000.dwc3 androidboot.init_fatal_reboot_target=recovery androidboot.verifiedbootstate=orange androidboot.keymaster=1 androidboot.bootdevice=1d84000.ufshc androidboot.fstab_suffix=default androidboot.boot_devices=soc/1d84000.ufshc androidboot.serialno=a2f162b1 androidboot.ramdump=disable androidboot.secureboot=1 androidboot.hwversion=8.9.0 androidboot.cpuid=0xb07eb700 androidboot.hwc=CN androidboot.hwlevel=MP androidboot.baseband=apq androidboot.ufsid=0x145 androidboot.slot_suffix=_b androidboot.dtbo_idx=10 androidboot.dp=0x0 androidboot.dtb_idx=0 androidboot.batt_verified_result=1111"
boot.configs.aaa.extra.use_uefi = false
boot.configs.aaa.extra.skip_efi_memory_map = true

boot.default = "aaa"
boot.second = "simple-init"
boot.uefi_probe = false
boot.title = "选择操作系统"
boot.timeout = 5
boot.timeout_text = "倒计时: %d"
sw = 8
locates.part_logfs.by_disk_label = "gpt"
locates.part_logfs.by_gpt_name = "logfs"
locates.part_esp.by_disk_label = "gpt"
locates.part_esp.by_gpt_name = "esp"
gui.show_background = true
gui.background = ""
gui.dark = true
w = 8
```

## 配置 UEFI 启动项

我们选择直接把 simple init 放到 `/EFI/BOOT/BOOTAA64.EFI`

## 问题解决

仅针对 nabu

### simple init 启动崩溃

simple init 与 release 构建的 Project Aloha 工作有问题 使用 debug 构建的

### uefi 没有触摸输入

用按键

### uefi 没有 USB 主机

可能它就没有吧

所以其实只有三个 GPIO 能输入

### kernel panic: Unable to handle kernel paging request at virtual address xxxx

```dmesg
<6>[    0.357234] Trying to unpack rootfs image as initramfs...
<1>[    0.360544] Unable to handle kernel paging request at virtual address ffffffc024c00000
```

simple init 传的 initrd 似乎有问题

太大会出问题

考虑使用更高效的压缩减小体积

不要用内嵌 initramfs (/ -> General setup -> Initial RAM filesystem and RAM disk (initramfs/initrd) support -> Initramfs source file(s))
它和你预期的不一样


### Android 无法启动

设备加密不工作

格式化 userdata 并重试


## 调试相关

kernel panic 后日志可以在`/sys/fs/pstore`里找到

重启后进 recovery 没法 su 的话 可以把 twrp 刷到 boot 分区

kernel 不 panic 的话 可以手动给它 panic

```c
#include <linux/module.h>
#include <linux/kernel.h>
#include <linux/delay.h>
#include <linux/kthread.h>

struct task_struct* task;

static int mythread(void* data) {
    int timeout = CONFIG_FORCE_PANIC_DELAY;
    printk(KERN_INFO "force_panic: thread\n");
    while (timeout > 0) {
        printk(KERN_INFO "force_panic: in %ds...\n", timeout);
        timeout--;
        ssleep(1);
    }
    panic(KERN_INFO "force_panic now\n");
    return 0;
}

static int myinit(void) {
    printk(KERN_INFO "force_panic: init\n");
    task = kthread_run(mythread, NULL, "force_panic");
    if (IS_ERR(task)) {
        panic(KERN_ERR "force_panic thread creation failed\n");
    }
    return 0;
}

static void myexit(void) {
    pr_info("panic cleanup\n");
}

module_init(myinit)
module_exit(myexit)
```
