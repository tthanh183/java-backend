### Synchronized Static Methods in Java

In Java, you can use the `synchronized` keyword to synchronize a static method. When a static method is synchronized, the lock is acquired on the class object, not on the instance of the class.  
The syntax of a synchronized static method is:
```java
public static synchronized void methodName() {
    // code to be synchronized
}
```

> [!NOTE]  
> If there are multiple instances of a class, only one thread can execute the synchronized static method at a time.
> If there is only one instance of a class, the synchronized static method behaves the same as a synchronized instance method.

Example:
```java
class Table {

    synchronized static void printTable(int n) {
        for (int i = 1; i <= 10; i++) {
            System.out.println(n * i);
            try {
                Thread.sleep(400);
            } catch (Exception e) {
            }
        }
    }
}

class MyThread1 extends Thread {
    public void run() {
        Table.printTable(1);
    }
}

class MyThread2 extends Thread {
    public void run() {
        Table.printTable(10);
    }
}

class MyThread3 extends Thread {
    public void run() {
        Table.printTable(100);
    }
}

class MyThread4 extends Thread {
    public void run() {
        Table.printTable(1000);
    }
}

public class TestSynchronization4 {
    public static void main(String t[]) {
        MyThread1 t1 = new MyThread1();
        MyThread2 t2 = new MyThread2();
        MyThread3 t3 = new MyThread3();
        MyThread4 t4 = new MyThread4();
        t1.start();
        t2.start();
        t3.start();
        t4.start();
    }
}
```
Output:
```
Count: 0
```






