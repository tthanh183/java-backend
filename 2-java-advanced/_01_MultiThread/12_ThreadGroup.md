### Thread Group

Java provides a convenient way to group multiple threads into a single object. This allows you to suspend, resume, or interrupt a group of threads by calling a single method.

> [!NOTE]  
> The methods suspend(), resume(), and stop() have been deprecated (no longer in use).

Thread groups in Java are implemented by the class java.lang.ThreadGroup.

The ThreadGroup class provides the following constructors:
- `ThreadGroup(String name)`: Creates a new thread group with the specified name.
- `ThreadGroup(ThreadGroup parent, String name)`: Creates a new thread group with the specified parent and name.

Important methods of the ThreadGroup class:
- `public int activeCount()`: Returns the number of active threads in the thread group.
- `public int activeGroupCount()`: Returns the number of active thread groups in the thread group.
- `public void list()`: Prints information about the thread group to the standard output.
- `public void destroy()`: Destroys the thread group and all its subgroups.
- `public String getName()`: Returns the name of the thread group.
- `public ThreadGroup getParent()`: Returns the parent of the thread group.
- `public void interrupt()`: Interrupts all threads in the thread group.

Example:
```java
public class ThreadGroupExample {
    public static void main(String[] args) {
        ThreadGroup group = new ThreadGroup("Group A");

        Thread t1 = new Thread(group, new MyRunnable(), "Thread 1");
        Thread t2 = new Thread(group, new MyRunnable(), "Thread 2");

        t1.start();
        t2.start();

        System.out.println("Active threads in the group: " + group.activeCount());
        System.out.println("Active thread groups in the group: " + group.activeGroupCount());

        group.list();
    }
}
```
Output:
```
Active threads in the group: 2
Active thread groups in the group: 0
java.lang.ThreadGroup[name=Group A,maxpri=10]
    Thread[Thread 1,5,Group A]
    Thread[Thread 2,5,Group A]
```