---
title: 缓解坏了的 wlan (2)
date: 2023-12-20 09:32:19
tags: [android]
excerpt: 好像解决 system anr 了
---

## 过程

瞎 hook 一下 尝试接管 wifi 服务

```kotlin
loadClass("android.os.ServiceManagerProxy")
    .findMethod { name == "addService" }
    .hookAfter {
        val name = it.args[0] as String
        if ("wifi" in name) {
            Log.d(TAG, "addService: find $name ${it.args[1]}")
        }
        if (name == "wifi") {
            val class_WifiServiceImpl = it.args[1].javaClass
            Log.d(TAG, "addService: get $class_WifiServiceImpl")
            handle_WifiServiceImpl(class_WifiServiceImpl)
        }
    }

data class WifiState(
    var enabled: Boolean = false,
)

val wifiState = WifiState()

fun handle_WifiServiceImpl(clazz: Class<*>) {
    clazz
        .findMethod { name == "setWifiEnabledInternal" }
        .hookReplace {
            wifiState.enabled = it.args[1] as Boolean
            Unit
        }
    clazz
        .findMethod { name == "getWifiEnabledState" }
        .hookReplace {
            return@hookReplace if (wifiState.enabled) /* WIFI_STATE_ENABLED */ 3 else /* WIFI_STATE_DISABLED */ 1
        }
}
```

然后它 anr 就没了

很好 不管它了