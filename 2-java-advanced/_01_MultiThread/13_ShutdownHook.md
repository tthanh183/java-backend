### Shutdown Hook 

Shutdown Hook can be used to perform cleanup of resources or save the state when the JVM shuts down either normally or abruptly. Cleaning up resources means closing log files, sending some alerts, or doing something else. Therefore, if you want to execute some code before the JVM shuts down, you can use a shutdown hook.

When does the JVM shut down? 
- When the user presses `Ctrl+C` in the console.
- The `System.exit(int)` method is called.
- The user logs off.
- The user shuts down the system, etc.

The addShutdownHook(Thread hook) method of the Runtime class is used to register a thread with the Virtual Machine. The syntax is:
```java
public void addShutdownHook(Thread hook) {}
```
You can create an instance of the Runtime class by calling the static method getRuntime(). Example:
```java
Runtime runtime = Runtime.getRuntime();
```

Example:
```java
public class ShutdownHookExample {
    public static void main(String[] args) {
        Runtime runtime = Runtime.getRuntime();
        runtime.addShutdownHook(new Thread(() -> {
            System.out.println("Shutdown hook is running!");
        }));
        System.out.println("Main thread is sleeping...");
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```
Output:
```
Main thread is sleeping...
Shutdown hook is running!
```
