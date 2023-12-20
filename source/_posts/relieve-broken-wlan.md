---
title: 缓解坏了的 wlan
date: 2023-12-20 04:11:54
tags: [android]
excerpt: "tun socks5 in adb/usb :thinking:"
---

## 起因

这手机 wlan 坏了 大概这么有坏

![broken1](broken1.png)

<details>
<summary>dmesg | grep wlan</summary>
```
OnePlus9:/ # dmesg | grep wlan
[    1.247929] cnss: Got regulator: vdd-wlan-io, min_uv: 1800000, max_uv: 1800000, load_ua: 0, delay_us: 0, need_unvote: 1
[    1.248194] cnss: Got regulator: wlan-ant-switch, min_uv: 2800000, max_uv: 2800000, load_ua: 0, delay_us: 0, need_unvote: 0
[    1.248273] cnss: Got regulator: vdd-wlan-aon, min_uv: 950000, max_uv: 952000, load_ua: 0, delay_us: 0, need_unvote: 1
[    1.248321] cnss: Got regulator: vdd-wlan-dig, min_uv: 950000, max_uv: 952000, load_ua: 0, delay_us: 0, need_unvote: 1
[    1.248370] cnss: Got regulator: vdd-wlan-rfa1, min_uv: 1880000, max_uv: 2000000, load_ua: 0, delay_us: 0, need_unvote: 1
[    1.248414] cnss: Got regulator: vdd-wlan-rfa2, min_uv: 1350000, max_uv: 1350000, load_ua: 0, delay_us: 0, need_unvote: 1
[    1.248815] cnss: Regulator vdd-wlan-io is being enabled
[    1.248907] cnss: Regulator wlan-ant-switch is being enabled
[    1.249266] cnss: Regulator vdd-wlan-aon is being enabled
[    1.249846] cnss: Regulator vdd-wlan-dig is being enabled
[    1.249946] cnss: Regulator vdd-wlan-rfa1 is being enabled
[    1.250040] cnss: Regulator vdd-wlan-rfa2 is being enabled
[    1.335898] ipa 1e00000.qcom,ipa:ipa_smmu_wlan: Adding to iommu group 44
[    1.427134] cnss: Regulator vdd-wlan-rfa2 is being disabled
[    1.427147] cnss: Regulator vdd-wlan-rfa1 is being disabled
[    1.427160] cnss: Regulator vdd-wlan-dig is being disabled
[    1.427185] cnss: Regulator vdd-wlan-aon is being disabled
[    1.427199] cnss: Regulator wlan-ant-switch is being disabled
[    1.427209] cnss: Regulator vdd-wlan-io is being disabled
[    1.628028] cnss: Regulator vdd-wlan-io is being enabled
[    1.628031] cnss: Regulator wlan-ant-switch is being enabled
[    1.628340] cnss: Regulator vdd-wlan-aon is being enabled
[    1.628891] cnss: Regulator vdd-wlan-dig is being enabled
[    1.628957] cnss: Regulator vdd-wlan-rfa1 is being enabled
[    1.629022] cnss: Regulator vdd-wlan-rfa2 is being enabled
[    1.806840] cnss: Regulator vdd-wlan-rfa2 is being disabled
[    1.806848] cnss: Regulator vdd-wlan-rfa1 is being disabled
[    1.806870] cnss: Regulator vdd-wlan-dig is being disabled
[    1.806892] cnss: Regulator vdd-wlan-aon is being disabled
[    1.806913] cnss: Regulator wlan-ant-switch is being disabled
[    1.806919] cnss: Regulator vdd-wlan-io is being disabled
[    2.208034] cnss: Regulator vdd-wlan-io is being enabled
[    2.208036] cnss: Regulator wlan-ant-switch is being enabled
[    2.208349] cnss: Regulator vdd-wlan-aon is being enabled
[    2.208898] cnss: Regulator vdd-wlan-dig is being enabled
[    2.208962] cnss: Regulator vdd-wlan-rfa1 is being enabled
[    2.209025] cnss: Regulator vdd-wlan-rfa2 is being enabled
[    2.386820] cnss: Regulator vdd-wlan-rfa2 is being disabled
[    2.386828] cnss: Regulator vdd-wlan-rfa1 is being disabled
[    2.386840] cnss: Regulator vdd-wlan-dig is being disabled
[    2.386862] cnss: Regulator vdd-wlan-aon is being disabled
[    2.386884] cnss: Regulator wlan-ant-switch is being disabled
[    2.386901] cnss: Regulator vdd-wlan-io is being disabled
[    3.008062] cnss: Regulator vdd-wlan-io is being enabled
[    3.008063] cnss: Regulator wlan-ant-switch is being enabled
[    3.008384] cnss: Regulator vdd-wlan-aon is being enabled
[    3.008942] cnss: Regulator vdd-wlan-dig is being enabled
[    3.009007] cnss: Regulator vdd-wlan-rfa1 is being enabled
[    3.009079] cnss: Regulator vdd-wlan-rfa2 is being enabled
[    3.186677] cnss: Regulator vdd-wlan-rfa2 is being disabled
[    3.186683] cnss: Regulator vdd-wlan-rfa1 is being disabled
[    3.186704] cnss: Regulator vdd-wlan-dig is being disabled
[    3.186726] cnss: Regulator vdd-wlan-aon is being disabled
[    3.186747] cnss: Regulator wlan-ant-switch is being disabled
[    3.186753] cnss: Regulator vdd-wlan-io is being disabled
[    3.200200] cnss: Put regulator: vdd-wlan-io
[    3.200214] cnss: Put regulator: wlan-ant-switch
[    3.200223] cnss: Put regulator: vdd-wlan-aon
[    3.200231] cnss: Put regulator: vdd-wlan-dig
[    3.200239] cnss: Put regulator: vdd-wlan-rfa1
[    3.200246] cnss: Put regulator: vdd-wlan-rfa2
[    3.503721] init: Parsing file /vendor/etc/init/init.vendor.wlan.rc...
[    6.044606] init: processing action (early-boot) from (/vendor/etc/init/init.vendor.wlan.rc:7)
[    6.382642] init: starting service 'exec 21 (/vendor/bin/modprobe -a -d /vendor/lib/modules/ qca_cld3_wlan qca_cld3_qca6390)'...
[    6.386186] init: ... started service 'exec 21 (/vendor/bin/modprobe -a -d /vendor/lib/modules/ qca_cld3_wlan qca_cld3_qca6390)' has pid 1203
[    6.386622] init: starting service 'exec 22 (/vendor/bin/modprobe -a -d /vendor/lib/modules/5.4-gki qca_cld3_wlan qca_cld3_qca6390)'...
[    6.393909] init: ... started service 'exec 22 (/vendor/bin/modprobe -a -d /vendor/lib/modules/5.4-gki qca_cld3_wlan qca_cld3_qca6390)' has pid 1236
[    6.412571] init: Service 'exec 22 (/vendor/bin/modprobe -a -d /vendor/lib/modules/5.4-gki qca_cld3_wlan qca_cld3_qca6390)' (pid 1236) exited with status 1 oneshot service took 0.019000 seconds in background
[    6.412578] init: Sending signal 9 to service 'exec 22 (/vendor/bin/modprobe -a -d /vendor/lib/modules/5.4-gki qca_cld3_wlan qca_cld3_qca6390)' (pid 1236) process group...
[    6.498882] wlan: module is from the staging directory, the quality is unknown, you have been warned.
[    6.540708] wlan_hdd_state wlan major(474) initialized
[    6.550377] init: Service 'exec 21 (/vendor/bin/modprobe -a -d /vendor/lib/modules/ qca_cld3_wlan qca_cld3_qca6390)' (pid 1203) exited with status 1 oneshot service took 0.165000 seconds in background
[    6.550385] init: Sending signal 9 to service 'exec 21 (/vendor/bin/modprobe -a -d /vendor/lib/modules/ qca_cld3_wlan qca_cld3_qca6390)' (pid 1203) process group...
[   11.902123] wlan: Loading driver v2.0.8.33V +PANIC_ON_BUG
[   11.902848] wlan_pld:pld_register_driver:296:: Fail to register pcie driver
```
</details>

`wlan_pld:pld_register_driver:296:: Fail to register pcie driver` 看上去就很寄

造成的影响包括但不限于

- 没有熟悉的手机用
- 无法使用 wlan
- 时常报告 system 无响应
- 无法使用 nearby share
- 没有 android 14 的开发机用、

## 解决方案

### 1. usb 以太网

操作
- 插个 usb 网卡就行了

工作 但在我工作区不适用 没有 rj45 接口

### 2. 蓝牙网络共享

操作
- 连接蓝牙
- 打开 互联网链接 选项

工作 但是速度很慢 因为蓝牙 5.0 可提供**高达** 2Mbps 的链接速度

### 3. usb 网络共享(RNDIS)

操作
- 连接 usb
- 打开 usb 网络共享

不工作 因为内核没有 RNDIS 驱动

### 4. tun socks5 in adb/usb

操作
- 连接 adb/usb
- 电脑 sing-box 里面跑一个 socks5 代理监听 tcp 10000
- `adb reverse tcp:10000 tcp:10000`
- 手机 nekobox 连接 socks5 127.0.0.1:10000

工作 还行但存在问题
- 部分应用不走 tun 网卡 (termux etc...)
- 活动依旧受限于数据线长度
- adb 断开后需要重新配置

## 最终方案

结合 2 和 4

所以支持的应用会走 adb/usb 不支持的回退到蓝牙

## 踩坑

不要在镜像网络模式的 wsl 里跑代理 性能差到离谱

<details>
<summary>iperf3 测试详细</summary>

```
# wsl
❯ iperf3 -s
-----------------------------------------------------------
Server listening on 5201 (test #1)
-----------------------------------------------------------
Accepted connection from 127.0.0.1, port 64330
[  5] local 127.0.0.1 port 5201 connected to 127.0.0.1 port 64331
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-1.00   sec   154 MBytes  1.29 Gbits/sec
[  5]   1.00-2.00   sec   161 MBytes  1.35 Gbits/sec
[  5]   2.00-3.00   sec   165 MBytes  1.38 Gbits/sec
[  5]   3.00-4.00   sec   167 MBytes  1.40 Gbits/sec
[  5]   4.00-5.00   sec   167 MBytes  1.40 Gbits/sec
[  5]   5.00-6.00   sec   166 MBytes  1.39 Gbits/sec
[  5]   6.00-7.00   sec   167 MBytes  1.40 Gbits/sec
[  5]   7.00-8.00   sec   160 MBytes  1.34 Gbits/sec
[  5]   8.00-9.00   sec   175 MBytes  1.47 Gbits/sec
[  5]   9.00-10.00  sec   175 MBytes  1.47 Gbits/sec
[  5]  10.00-10.00  sec   384 KBytes  1.26 Gbits/sec
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-10.00  sec  1.62 GBytes  1.39 Gbits/sec                  receiver
-----------------------------------------------------------
Server listening on 5201 (test #2)
-----------------------------------------------------------
Accepted connection from 127.0.0.1, port 64342
[  5] local 127.0.0.1 port 5201 connected to 127.0.0.1 port 64343
[ ID] Interval           Transfer     Bitrate         Retr  Cwnd
[  5]   0.00-1.00   sec   214 KBytes  1.75 Mbits/sec   36   2.85 KBytes
[  5]   1.00-2.00   sec   150 KBytes  1.23 Mbits/sec   34   2.85 KBytes
[  5]   2.00-3.00   sec   147 KBytes  1.20 Mbits/sec   34   2.85 KBytes
[  5]   3.00-4.00   sec   218 KBytes  1.79 Mbits/sec   46   2.85 KBytes
[  5]   4.00-5.00   sec   220 KBytes  1.80 Mbits/sec   44   2.85 KBytes
[  5]   5.00-6.00   sec   220 KBytes  1.80 Mbits/sec   34   2.85 KBytes
[  5]   6.00-7.00   sec   144 KBytes  1.18 Mbits/sec   42   2.85 KBytes
[  5]   7.00-8.00   sec   222 KBytes  1.82 Mbits/sec   36   7.13 KBytes
[  5]   8.00-9.00   sec   217 KBytes  1.78 Mbits/sec   50   2.85 KBytes
[  5]   9.00-10.00  sec   151 KBytes  1.24 Mbits/sec   32   2.85 KBytes
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bitrate         Retr
[  5]   0.00-10.01  sec  1.86 MBytes  1.56 Mbits/sec  388             sender
```

</details>

大概 `adb/usb -> windows -> wsl` 上行有 1.39 Gbits/sec 还看得过去

但 `wsl -> windows -> adb/usb` 下行只有 1.56 Mbits/sec 实属离谱

## 反思

- 所以为什么不去修
  - 因为不敢去 社恐

## 感谢

[咕咕华](https://github.com/cinit) 不说我都忘了 vpn 是 tun 和 adb 能转发接口了