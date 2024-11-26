### Create a thread in Java

In Java, you can create a thread in two ways:
- By extending the Thread class
- By implementing the Runnable interface

1. **Extending the Thread class**  

The Thread class provides constructors and methods to create and manage threads. The Thread class extends from the Object class and implements the Runnable interface.

Common Constructors of the Thread class:
- Thread()
- Thread(String name)
- Thread(Runnable target)
- Thread(Runnable target, String name)
- Thread(ThreadGroup group, Runnable target)
- Thread(ThreadGroup group, Runnable target, String name)

Common methods of the Thread class:
- public void start(): starts the thread. The JVM calls the run() method on the thread.
- public void run(): contains the code that the thread will execute
- public void sleep(long milliseconds): causes the currently executing thread to sleep for the specified number of milliseconds
- public void join(): waits for the thread to die
- public void join(long milliseconds): waits for the thread to die for the specified number of milliseconds
- public int getPriority(): returns the priority of the thread
- public int setPriority(int priority): changes the priority of the thread
- public String getName(): returns the name of the thread
- public void setName(String name): changes the name of the thread
- public Thread currentThread(): returns the reference of the currently executing thread
- public int getId(): returns the ID of the thread
- public Thread.State getState(): returns the state of the thread
- public boolean isAlive(): tests if the thread is alive

2. **Implementing the Runnable interface**

The Runnable interface should be implemented by any class whose instances are intended to be executed by a thread. The class must define a method of no arguments called run.

Common methods of the Runnable interface:
- public void run(): contains the code that the thread will execute

3. **Starting a thread**

The start() method is used to start a thread. It performs the following tasks:
- A new thread is created
- The thread moves from the new state to the runnable state
- When the thread gets a chance to execute, its target run() method will run

Example of creating a thread by extending the Thread class:
```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread is running...");
    }

    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.start();
    }
}
```

Example of creating a thread by implementing the Runnable interface:
```java
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Thread is running...");
    }

    public static void main(String[] args) {
        MyRunnable myRunnable = new MyRunnable();
        Thread t1 = new Thread(myRunnable);
        t1.start();
    }
}
```