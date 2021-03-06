# How to detect deadlock?

You can do this programmatically using the ThreadMXBean that ships with JDK.

```java
ThreadMXBean bean = ManagementFactory.getThreadMXBean();
long[] threadIds = bean.findDeadlockedThreads(); // Returns null if no threads are deadlocked.

if (threadIds != null) {
  ThreadInfo[] infos = bean.getThreadInfo(threadIds);

  for (ThreadInfo info : infos) {
    StackTraceElement[] stack = info.getStackTrace();
    // Log or store stack trace information.
  }
}
```

Obviously you should try to isolate whichever thread is performing this deadlock check - Otherwise if that thread deadlocks it won't be about to run the check!

Incidentlly this is what JConsole is using under the covers.


Another method is findMonitorDeadlockedThreads. Differences are the second will detect synchronized lock only, the first one could detect owner lock.

Sometimes the timeout to get the lock will also treated as deadlock, this is open to investigate.
