### Start a thread twice
```java
public class StartThreadTwice extends Thread {
    public void run() {
        System.out.println("running...");
    }

    public static void main(String[] args) {
        StartThreadTwice t1 = new StartThreadTwice();
        t1.start();
        t1.start();
    }
}
```
Output:
```
Exception in thread "main" java.lang.IllegalThreadStateException
```
