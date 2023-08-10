# Android memory



## ADB command to monitor the Memory

```
adb shell dumpsys procstats --hours 3
adb shell dumpsys meminfo package_name|pid [-d]
adb shell cat /proc/meminfo
```

{% embed url="https://developer.android.com/tools/dumpsys#ViewingAllocations" %}

## Linux Kernel OOM Killer

the Android Out of Memory (OOM) killer, which is a part of the Linux kernel, has contributed to the Linux organization.



## **Android LMK vs Linux oomkiller**

LMKs on Android, whether the old in-kernel `lowmemkiller` or the newer `lmkd`, use a completely different mechanism than the standard [Linux kernel's OOM Killer](https://linux-mm.org/OOM\_Killer).&#x20;



LMK will try to kill in priority background applications, hidden applications or paused applications (it is connected to the ActivityManager of Android to know who is running and who is not). This way, it will let user continue to use his current application and kill other apps. LMK most priority is to let user use smoothly his application. Secondly, LMK will in general avoid to kill system applications, preferring user applications and letting system running.

OOM will try to kill in priority applications that use most of the memory, without any concerns about the fact that this application was currently used by user, what OOM wants to do is to keep the whole system "safe" and running well (user ? who cares ? ;) ). Yet, it can decide to kill some system daemons that was useful for the system but was the biggest "memory eater". OOM killer algorithm is based on the `oom_score` which used to be computed on very complicated heuristics and is now mostly based on the memory percentage consumed.

## Android LMK

The Android framework kills apps and services, especially background ones, to make room for newly opened apps when memory is needed. These are known as low memory kills (LMK).

LMK will check system by some interval, when no enough memory, LMK will tigger to kill process according to LMK logic.

{% embed url="https://android.googlesource.com/platform/external/perfetto/+/refs/heads/master/docs/data-sources/memory-counters.md" %}

### lowmemorykiller vs lmkd

old android was using lowmemorykiller. android 9 start use lmkd. In Android 9 and later, userspace `lmkd` activates if an in-kernel LMK driver isn't detected. Because userspace `lmkd` requires kernel support for memory cgroups.





### init.rc config for lmkd

{% embed url="https://source.android.com/docs/core/perf/lmkd" %}

[https://cs.android.com/android/platform/superproject/+/master:system/core/rootdir/init.rc;l=32?q=pressure\_level\&sq=\&ss=android%2Fplatform%2Fsuperproject:system%2F](https://cs.android.com/android/platform/superproject/+/master:system/core/rootdir/init.rc;l=32?q=pressure\_level\&sq=\&ss=android%2Fplatform%2Fsuperproject:system%2F)

```
# memory.pressure_level used by lmkd
chown root system /dev/memcg/memory.pressure_level
chmod 0040 /dev/memcg/memory.pressure_level
# app mem cgroups, used by activity manager, lmkd and zygote
mkdir /dev/memcg/apps/ 0755 system system
# cgroup for system_server and surfaceflinger
mkdir /dev/memcg/system 0550 system system
```

### Android OOM levels

the value is changed by [https://cs.android.com/android/\_/android/platform/frameworks/base/+/fdcc4d4235c829a41b4d23043431bb8fa915a440](https://cs.android.com/android/\_/android/platform/frameworks/base/+/fdcc4d4235c829a41b4d23043431bb8fa915a440)

<pre><code><strong>OOM levels:
</strong>    -900: SYSTEM_ADJ (   49,152K)
    -800: PERSISTENT_PROC_ADJ (   49,152K)
    -700: PERSISTENT_SERVICE_ADJ (   49,152K)
      0: FOREGROUND_APP_ADJ (   49,152K)
     100: VISIBLE_APP_ADJ (   61,440K)
     200: PERCEPTIBLE_APP_ADJ (   73,728K)
     225: PERCEPTIBLE_MEDIUM_APP_ADJ (   86,016K)
     250: PERCEPTIBLE_LOW_APP_ADJ (   86,016K)
     300: BACKUP_APP_ADJ (  147,456K)
     400: HEAVY_WEIGHT_APP_ADJ (  147,456K)
     500: SERVICE_ADJ (  147,456K)
     600: HOME_APP_ADJ (  147,456K)
     700: PREVIOUS_APP_ADJ (  147,456K)
     800: SERVICE_B_ADJ (  147,456K)
     900: CACHED_APP_MIN_ADJ (  147,456K)
     999: CACHED_APP_MAX_ADJ (  215,040K)
    
</code></pre>

```
adb shell
cd /proc/pid/
oom_adj oom_score oom_score_adj
```

how lmk use oom\_score\_adj: before Android 7.0, oom\_score\_adj=oom\_adj\*1000/17; after 7.0 oom\_score\_adj=oom\_adj

{% embed url="https://www.droidviews.com/tweak-android-low-memory-killer-needs/" %}

Each of the categories above has its own memory threshold and when free RAM falls below this threshold, LMK will start killing processes inside this category, starting with processes with lower priorities.

## Android OOM adjuster

{% embed url="https://android.googlesource.com/platform/frameworks/base/+/master/services/core/java/com/android/server/am/OomAdjuster.md" %}

{% embed url="https://blog.csdn.net/yun_hen/article/details/123778440" %}

## [https://www.xda-developers.com/hidden-changes-android-11-source-code/](https://www.xda-developers.com/hidden-changes-android-11-source-code/)

(OOM = out-of-memory, i.e., what should the system do when the amount of free memory is close to depleted). There are 3 factors for OOM Adjuster tweaks: Process State (determine if a process is in the foreground versus background), OOM Adj score (used by the low memory killer daemon, or lmkd, to determine which process should be killed when low on memory), and the Scheduler Group (which tweaks the CPU process group and thread priorities).

The system server adjusts these 3 factors for 4 types of different Android processes: Activity, Service, Content Provider, and Broadcast Receiver. OOM Adjuster is designed to avoid killing a process if _"it would result \[in a] user-perceptible interruption of service."_

{% embed url="http://strayinsights.blogspot.com/search/label/Android%20Memory%20Overview" %}
Android Memory Overview
{% endembed %}
