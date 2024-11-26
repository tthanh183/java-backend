### Priority of a Thread

Each thread has a priority level, which is represented by a number from 1 to 10. In most cases, the thread scheduling is done according to their priority levels (called priority scheduling). However, this is not guaranteed because it depends on the JVM specifications.

3 constants are defined in the Thread class to represent the priority levels:
- `public static int MIN_PRIORITY`: The minimum priority that a thread can have. The value of this constant is 1.
- `public static int NORM_PRIORITY`: The default priority level. The value of this constant is 5.
- `public static int MAX_PRIORITY`: The maximum priority that a thread can have. The value of this constant is 10.

Example:
```java
public class PriorityThread extends Thread {
    public void run() {
        System.out.println("running...");
    }

    public static void main(String[] args) {
        PriorityThread t1 = new PriorityThread();
        PriorityThread t2 = new PriorityThread();

        System.out.println("Priority of t1: " + t1.getPriority());
        System.out.println("Priority of t2: " + t2.getPriority());

        t1.setPriority(Thread.MIN_PRIORITY);
        t2.setPriority(Thread.MAX_PRIORITY);

        System.out.println("Priority of t1 after changing: " + t1.getPriority());
        System.out.println("Priority of t2 after changing: " + t2.getPriority());

        t1.start();
        t2.start();
    }
}
```
Output:
```
Priority of t1: 5
Priority of t2: 5
Priority of t1 after changing: 1
Priority of t2 after changing: 10
running...
running...
```
