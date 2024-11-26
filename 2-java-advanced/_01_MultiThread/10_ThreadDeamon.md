### Thread Daemon 

A daemon thread in Java is a thread that provides services for user threads. Its lifetime depends on user threads, meaning when all user threads die, the JVM will automatically terminate this thread.

There are several daemon threads in Java that run automatically, such as garbage collection (GC), finalizer threads, etc.

Key points about daemon threads:
- A daemon thread is a low-priority thread that runs in the background.
- It provides services to user threads.
- It depends on user threads. If all user threads die, the JVM will automatically terminate this thread.

Why does JVM terminate a daemon thread when all user threads die?  
The sole purpose of a daemon thread is to provide services for user threads for background tasks. If there are no user threads, why would the JVM continue to run this thread? That is why the JVM terminates daemon threads when there are no user threads being executed.

Methods of Daemon thread:
- `public void setDaemon(boolean on)`: Used to set a thread as a daemon thread or a user thread. The default value is false.
- `public boolean isDaemon()`: Used to check if a thread is a daemon thread or a user thread.

Example:
```java
public class DaemonThread extends Thread {
    public void run() {
        if (Thread.currentThread().isDaemon()) {
            System.out.println("Daemon thread running...");
        } else {
            System.out.println("User thread running...");
        }
    }

    public static void main(String[] args) {
        DaemonThread t1 = new DaemonThread();
        DaemonThread t2 = new DaemonThread();

        t1.setDaemon(true);

        System.out.println("Is t1 a daemon thread? " + t1.isDaemon());
        System.out.println("Is t2 a daemon thread? " + t2.isDaemon());

        t1.start();
        t2.start();
    }
}
```
Output:
```
Is t1 a daemon thread? true
Is t2 a daemon thread? false
Daemon thread running...
User thread running...
```


