### Synchronization in Java

Synchronization in Java is the ability to control access to shared resources by multiple threads.

For example, if multiple threads want to access the same variable at the same time, one thread may want to read it while another thread tries to modify it, which could lead to data inconsistency. In this case, Java Synchronization is a good option to ensure that only one thread can access that shared resource at any given time.

Why do we need synchronization in Java?
- To prevent interference between threads.
- To prevent consistency problems.

Problems that can occur without synchronization:
```java
public class Counter {
    private int count = 0;

    public void increment() {
        count++;
    }

    public void decrement() {
        count--;
    }

    public int getCount() {
        return count;
    }
}

public class SynchronizationExample {
    public static void main(String[] args) {
        Counter counter = new Counter();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.decrement();
            }
        });

        t1.start();
        t2.start();

        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Count: " + counter.getCount());
    }
}
```
Output:
```
Count: -1000
```
In the above example, we have a `Counter` class with three methods: `increment()`, `decrement()`, and `getCount()`. We have two threads `t1` and `t2` that increment and decrement the count variable, respectively. When we run the program, we expect the count to be 0 because `t1` increments the count by 1000 and `t2` decrements the count by 1000. However, the output is `-1000` because both threads are accessing the `count` variable at the same time, which leads to data inconsistency.

To solve this problem, we can use the `synchronized` keyword in Java.
> [!NOTE]  
> The `synchronized` keyword in Java is used to create a synchronized block of code. The synchronized block ensures that only one thread can execute the synchronized code at a time.
```java
public class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public synchronized void decrement() {
        count--;
    }

    public synchronized int getCount() {
        return count;
    }
}

public class SynchronizationExample {
    public static void main(String[] args) {
        Counter counter = new Counter();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.decrement();
            }
        });

        t1.start();
        t2.start();

        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Count: " + counter.getCount());
    }
}
```
Output:
```
Count: 0
```
