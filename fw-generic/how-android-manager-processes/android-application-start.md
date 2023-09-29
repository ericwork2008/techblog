# Android Application start

[Android O系统启动流程--zygote篇 - Vane的博客 | Vane's Blog](http://vanelst.site/2019/11/07/android-startup-zygote/)

前面说过在Android应用服务框架启动中，Zygote会进入一个消息循环来监听发往zygote socket上的消息。

> frameworks/base/core/java/com/android/internal/os/ZygoteInit.java：

ZygoteConnection.java负责zygote socket的连接管理，其它Activity就是通过socket发送请求到Zygote服务上，Zygote通过Socket接收ActivityManangerService的请求，Fork应用程序。

ActivityManangerService中startProcessLocked（）用来启动进程。

Process.java中类Process函数start可以用来启动进程。startViaZygote会向zygote发送消息，然后zygote会fork一个新的进程。

#### App start detail <a href="#app-start-detail" id="app-start-detail"></a>

[https://developer.android.com/topic/performance/vitals/launch-time](https://developer.android.com/topic/performance/vitals/launch-time)
