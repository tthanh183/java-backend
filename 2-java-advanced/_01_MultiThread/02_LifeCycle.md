### Life Cycle of a Thread

A thread can exist in one of five states. According to Sun, there are only 4 states in the life cycle of a thread in Java: new, runnable, non-runnable, and terminated. There is no "running" state explicitly.

However, to better understand threads, we will look at the thread in 5 states.

The life cycle of a thread in Java is controlled by the JVM. The states of a thread in Java are as follows:

1. New  

A thread is in the new state if you create an instance of the Thread class but before calling the start() method.
2. Runnable

A thread is in the runnable state after calling the start() method, but the thread scheduler has not yet selected it as the currently running thread.

3. Running

A thread is in the running state if the thread scheduler has selected it to run.

4. Non-Runnable

This is the state when the thread is still alive but is not selected to run at the moment.

5. Terminated

A thread is in the terminated or dead state when the run() method has completed and it has finished its execution.

The following diagram shows the life cycle of a thread:

![Thread Life Cycle](https://viettuts.vn/images/java/java-thread/vong-doi-cua-thread-trong-java.jpg)