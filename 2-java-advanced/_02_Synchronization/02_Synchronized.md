### Synchronized Blocks in Java

A synchronized block in Java can be used to synchronize specific resources within a method.

Imagine a method with 50 lines of code, but you only want to synchronize 5 lines; you can use a synchronized block for this purpose.

If you place all the lines of code in a synchronized block, it will behave the same as a synchronized method.

Key points about synchronized blocks:
- A synchronized block is used to lock an object for any shared resource.
- The scope of the synchronized block is smaller than that of a synchronized method.
- The synchronized block is more efficient than a synchronized method because it reduces the scope of the lock.
- The synchronized block must be inside a method or a code block.  
The syntax of a synchronized block is:
```
synchronized (object) {
    // code to be synchronized
}
```

Example:
```java
public class SynchronizedBlockExample {
    private int count = 0;
    private Object lock = new Object();

    public void increment() {
        synchronized (lock) {
            count++;
        }
    }

    public void decrement() {
        synchronized (lock) {
            count--;
        }
    }

    public int getCount() {
        synchronized (lock) {
            return count;
        }
    }

    public static void main(String[] args) {
        SynchronizedBlockExample example = new SynchronizedBlockExample();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                example.increment();
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                example.decrement();
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

        System.out.println("Count: " + example.getCount());
    }
}
```
Output:
```
Count: 0
```
