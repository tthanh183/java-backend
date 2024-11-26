### Join Thread in Java

The join() method in Java is used to pause the current thread execution until the specified thread is dead. In other words, it waits for the specified thread to complete its execution.

The join() method is overloaded and has two versions:
- `public final void join() throws InterruptedException`: This method waits for the thread to die.
- `public final void join(long milliseconds) throws InterruptedException`: This method waits at most `milliseconds` for the thread to die.

Example:
```java
public class JoinThread extends Thread {
    public void run() {
        for (int i = 1; i <= 5; i++) {
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                System.out.println(e);
            }
            System.out.println(i);
        }
    }

    public static void main(String[] args) {
        JoinThread t1 = new JoinThread();
        JoinThread t2 = new JoinThread();
        JoinThread t3 = new JoinThread();

        t1.start();
        try {
            t1.join();
        } catch (InterruptedException e) {
            System.out.println(e);
        }

        t2.start();
        t3.start();
    }
}
```
Output:
``` 
1
2
3
4
5
1
1
2
2
3
3
4
4
5
5
```

In the above example, the main thread waits for `t1` to complete its execution before starting `t2` and `t3`.
