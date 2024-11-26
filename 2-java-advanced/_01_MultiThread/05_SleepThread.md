### Sleep a Thread

The sleep() method in the Thread class is used to temporarily pause a thread for a specified period of time.

```
public static void sleep(long milliseconds) throws InterruptedException
public static void sleep(long milliseconds, int nanoseconds) throws InterruptedException
```

Example:
```java
public class SleepThread extends Thread {
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
        SleepThread t1 = new SleepThread();
        SleepThread t2 = new SleepThread();

        t1.start();
        t2.start();
    }
}
```
Output:
```
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



