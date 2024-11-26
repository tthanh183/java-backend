### Thread Scheduler

A Thread Scheduler in Java, or the thread scheduling mechanism, is a part of the JVM responsible for deciding which thread should run.

There is no guarantee that a thread in the runnable state will be chosen to execute by the thread scheduler.

At any given time, only one thread can run on a single processor.

The thread scheduler mainly uses priority scheduling or time-sharing to manage and schedule threads.

Difference Between Priority Scheduling and Time Sharing

- Priority Scheduling: In this method, the task with the highest priority is executed until it either goes into a waiting state, a terminated state, or until a higher-priority task appears.
- Time Sharing: In this method, a task is executed for a predefined time slice, after which it is placed back into the pool of ready tasks. The scheduler determines which task to execute next based on priorities and other factors.