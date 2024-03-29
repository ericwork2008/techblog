---
description: Some are generated by ChatGPT
---

# How Android Manager Processes?

## Android Process

{% embed url="https://developer.android.com/guide/components/processes-and-threads" %}

The following link good writing.

{% embed url="https://blog.csdn.net/tonyandroid1984/article/details/68482901?spm=1001.2014.3001.5502" %}

in android when system start Activity/Provider/BroadcastReceiver/Service, it will check if there is an  existing process, if not will call startProcessLocked() to start it.



In Android, the `ProcessList` class is involved in various aspects of process management, such as maintaining the list of running processes, tracking their states, and managing process priorities. It interacts closely with other components of the Android system, such as the Activity Manager (AM), to perform these tasks.

Here are some key functions and responsibilities of the `ProcessList` class:

1. Tracking Running Processes: The `ProcessList` class keeps track of all the processes running in the system. It maintains a list of `ProcessRecord` objects, where each object represents a single process and holds information about its state, priority, and other relevant details.
2. Process Lifecycle Management: The class handles the lifecycle of processes, including starting and stopping them based on various events and triggers. It manages the creation of new processes and the termination of existing processes when necessary.
3. OOM (Out of Memory) Management: Android follows an OOM management mechanism to deal with resource constraints and prioritize processes based on their importance. The `ProcessList` class implements algorithms for calculating and adjusting process priorities in response to memory pressure, ensuring that critical processes are given higher priority and are less likely to be killed when memory is scarce.
4. Process Prioritization: Android assigns different priorities to processes based on factors like foreground or background status, user interactions, importance of the component being executed (such as an activity or service), and more. The `ProcessList` class handles the assignment and adjustment of process priorities, facilitating efficient resource allocation.
5. State Tracking and Updates: The class monitors the state of processes, such as whether they are in the foreground or background, sleeping, or waiting for events. It receives updates from various system components, such as the Activity Manager, and keeps the process states up to date.
6. Process Monitoring and Reaping: The `ProcessList` class periodically checks the state of processes and performs maintenance tasks like reaping zombie processes (terminated processes that have not been cleaned up) to ensure the system's stability and resource efficiency.

It's important to note that the implementation details and specific functionalities within the `ProcessList` class may vary between Android versions and device manufacturers, as Android is open source, and modifications can be made to the AOSP by different parties.



## "adb shell dumpsys activity processes"

{% embed url="https://blog.csdn.net/yun_hen/article/details/122876629?spm=1001.2014.3001.5502" %}

