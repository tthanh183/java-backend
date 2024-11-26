### Naming Thread

The Thread class provides methods to change and retrieve the name of a thread. By default, each thread has a name, such as thread-0, thread-1, etc. However, we can change the name of a thread using the setName() method. The syntax for setName() and getName() is as follows:

- public String getName(): Used to return the name of a thread.
- public void setName(String name): Used to change the name of a thread.

Example:
```java
public class NamingThread extends Thread {
    public void run() {
        System.out.println("running...");
    }

    public static void main(String[] args) {
        NamingThread t1 = new NamingThread();
        NamingThread t2 = new NamingThread();

        System.out.println("Name of t1: " + t1.getName());
        System.out.println("Name of t2: " + t2.getName());

        t1.start();
        t2.start();

        t1.setName("Thread 1");
        t2.setName("Thread 2");

        System.out.println("Name of t1 after changing: " + t1.getName());
        System.out.println("Name of t2 after changing: " + t2.getName());
    }
}
```
Output:
```
Name of t1: Thread-0
Name of t2: Thread-1
running...
running...
Name of t1 after changing: Thread 1
Name of t2 after changing: Thread 2
```

Example of using currentThread() method:
```java
public class NamingThread extends Thread {
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }

    public static void main(String[] args) {
        NamingThread t1 = new NamingThread();
        NamingThread t2 = new NamingThread();

        t1.start();
        t2.start();
    }
}
```
Output:
```
Thread-0
Thread-1
```



