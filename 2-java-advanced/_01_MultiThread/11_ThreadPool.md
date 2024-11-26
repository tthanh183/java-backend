### Thread Pool 

A thread pool in Java represents a group of threads that are waiting for tasks and are reused multiple times.

In the case of a thread pool, a fixed-size group of threads is created. A thread from the thread pool is pulled out and assigned a task by the service provider. Once the task is completed, the thread is put back into the thread pool.

Advantages of Thread Pool in Java

- Better performance: Saves time as there is no need to create a new thread.
- Real-time usage: It is used in Servlets and JSP, where the container creates a thread pool to handle requests.

Example of using a thread pool:

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ThreadPoolExample {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(5); // Create a thread pool with 5 threads

        for (int i = 0; i < 10; i++) {
            Runnable worker = new WorkerThread("" + i);
            executor.execute(worker);
        }

        executor.shutdown(); // Shutdown the executor
        while (!executor.isTerminated()) {
        }

        System.out.println("Finished all threads");
    }
}

class WorkerThread implements Runnable {
    private String message;

    public WorkerThread(String s) {
        this.message = s;
    }

    public void run() {
        System.out.println(Thread.currentThread().getName() + " (Start) message = " + message);
        processMessage(); // Call processMessage method that sleeps the thread for 2 seconds
        System.out.println(Thread.currentThread().getName() + " (End)");//prints thread name
    }

    private void processMessage() {
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```
Output:
```
pool-1-thread-1 (Start) message = 0
pool-1-thread-5 (Start) message = 4
pool-1-thread-4 (Start) message = 3
pool-1-thread-3 (Start) message = 2
pool-1-thread-2 (Start) message = 1
pool-1-thread-1 (End)
pool-1-thread-3 (End)
pool-1-thread-2 (End)
pool-1-thread-5 (End)
pool-1-thread-5 (Start) message = 6
pool-1-thread-2 (Start) message = 7
pool-1-thread-1 (Start) message = 8
pool-1-thread-4 (End)
pool-1-thread-3 (Start) message = 5
pool-1-thread-4 (Start) message = 9
pool-1-thread-2 (End)
pool-1-thread-4 (End)
pool-1-thread-3 (End)
pool-1-thread-5 (End)
pool-1-thread-1 (End)
Finished all threads
```





