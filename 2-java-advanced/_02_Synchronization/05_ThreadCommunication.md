### Thread Communication

Inter-thread communication in Java is a technique that allows threads to synchronize and communicate with each other.

Inter-thread communication is a mechanism in which one thread is temporarily paused during its critical section, and another thread is allowed to intervene (or lock) within the same critical section. This is achieved using the following methods from the Object class:
- `wait()`
- `notify()`
- `notifyAll()`

1. **`wait()` method**

wait() method is used to pause the current thread and release the lock. The thread will wait until another thread calls the notify() or notifyAll() method for the same object.
```
public final void wait() throws InterruptedException
public final void wait(long timeout) throws InterruptedException
```

> [!NOTE]  
> Lock means that the thread has exclusive access to the object. If a thread has a lock on an object, no other thread can access the object until the lock is released.
2. **`notify()` method**

notify() method is used to wake up a single thread that is waiting for the object lock. If multiple threads are waiting for the object lock, only one thread will be awakened.
```
public final void notify()
```
3. **`notifyAll()` method**

notifyAll() method is used to wake up all threads that are waiting for the object lock.
```
public final void notifyAll()
```

4. **Flow of Inter-thread Communication**

![Inter-thread Communication](https://viettuts.vn/images/java/java-thread/giao-tiep-giua-cac-luong-trong-java.jpg)

- A thread enters to acquire the lock.
- The lock is acquired by a thread.
- The thread then enters the waiting state if the `wait()` method is called on the object. Otherwise, it will release the lock and exit.
- If `notify()` or `notifyAll()` is called, the thread transitions to the notified state (ready to run).
- The thread is now available to acquire the lock.
- After completing its task, the thread releases the lock and exits the monitor state of the object.

Why are `wait()`, `notify()`, and `notifyAll()` methods in the Object class, not in the Thread class?
-> Because these methods are used to control the lock of an object, not the thread itself.

Example:
```java
class Customer {
    int amount = 10000;

    synchronized void withdraw(int amount) {
        System.out.println("Going to withdraw...");

        if (this.amount < amount) {
            System.out.println("Less balance; waiting for deposit...");
            try {
                wait();
            } catch (Exception e) {
                System.out.println(e);
            }
        }

        this.amount -= amount;
        System.out.println("Withdraw completed...");
    }

    synchronized void deposit(int amount) {
        System.out.println("Going to deposit...");
        this.amount += amount;
        System.out.println("Deposit completed...");
        notify();
    }
}

public class InterThreadCommunication {
    public static void main(String[] args) {
        final Customer c = new Customer();

        new Thread(() -> c.withdraw(15000)).start();
        new Thread(() -> c.deposit(10000)).start();
    }
}
```
Output:
```
Going to withdraw...
Less balance; waiting for deposit...
Going to deposit...
Deposit completed...
Withdraw completed...
```
