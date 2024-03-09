---
description: Draft by ChatGPT, then copy from Google MAD skills
---

# App Performance

![](<../.gitbook/assets/image (5) (1) (1).png>)

## Matrics

Certainly, when analyzing the performance of an Android app, it's essential to understand the different log messages and metrics, including "Fully Drawn" or "Displayed," and how they relate to the app's user experience. Here's an explanation of some key logs and metrics related to app performance:



### **Displayed Time (or Fully Drawn Time):**

1.  **TTID (Time to initial display)**

    _ActivityTaskManager：displayed_
2. **TTFD (Time to full display)**

_ActivityTaskManager：Fully drawn_

_Activity#reportFullyDrawn()_



<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

* **Log Message:** "Displayed com.example.package/.MainActivity: +XsXXXms"
* **Metric:** This log message appears in the Logcat and represents the time it took for the main activity (or a specific activity) to become visible to the user after the app was launched.
* **Significance:** The displayed time is a crucial metric for measuring the app's startup performance. It includes the time taken for activities to initialize, layout, and render their content. A lower displayed time indicates a more responsive app.
*   **Usage:** To log the displayed time, you can use a custom `Log` statement with timestamps at the start and end of your main activity's `onCreate` method:

    ```kotlin
    kotlinCopy codeval startTime = SystemClock.elapsedRealtime()
    // ... Your initialization code ...
    val endTime = SystemClock.elapsedRealtime()
    Log.d("Performance", "Displayed ${javaClass.simpleName}: +${endTime - startTime}ms")
    ```

### **Frame Rendering (FPS - Frames Per Second):**

* **Log Message:** Various log messages related to frame rendering may include "Choreographer", "SurfaceTexture", and "BufferQueue" logs.
* **Metric:** Frame rendering measures how smoothly the app updates the screen. It's represented in frames per second (FPS), and higher FPS values indicate smoother animations and interactions.
* **Significance:** Low FPS values can result in a janky and stuttery user experience. Monitoring frame rendering helps identify performance bottlenecks that affect UI smoothness.
* **Usage:** You can use Android's developer options to enable "Show layout bounds" and "Show GPU view updates" to visualize rendering issues on the screen. Additionally, tools like the Android Profiler or third-party tools like Systrace can help analyze frame rendering and identify performance issues.



<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### **GC (Garbage Collection) Logs:**

* **Log Messages:** Logs related to garbage collection events often start with "GC" or "Alloc".
* **Metric:** GC logs provide information about the garbage collection process, including when it occurs, how long it takes, and how much memory is collected or freed.
* **Significance:** Frequent or long-duration garbage collection events can lead to UI freezes and poor app performance. Monitoring GC logs helps you identify memory management issues.
* **Usage:** You can enable GC logging by adding options like `-Xlog:gc*` to your app's command line arguments or using Android Profiler's Memory Profiler to monitor memory allocation and GC events.

### **Network Logs:**

* **Log Messages:** Logs related to network requests and responses, including HTTP status codes and response times.
* **Metric:** Network logs provide information about the timing and success of network operations.
* **Significance:** Slow or failed network requests can impact app performance and user experience. Monitoring network logs helps diagnose issues with network connectivity and response times.
* **Usage:** You can use libraries like OkHttp or Retrofit to log network requests and responses. Additionally, Android Profiler's Network Profiler can help monitor network activity.

These are some of the key logs and metrics that can help you assess and improve your Android app's performance. By analyzing these logs and metrics, you can identify bottlenecks, optimize critical parts of your app, and deliver a smoother and more responsive user experience.

## Profiler Tools

```
<profileable>
```

SystemTracing

## Macrobenchmark

Add benchmark support

![](<../.gitbook/assets/image (4) (1).png>)



## Android Profiler's Network Profiler
