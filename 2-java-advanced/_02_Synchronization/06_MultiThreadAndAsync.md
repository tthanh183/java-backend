### Difference between Multi-threading and Asynchronous programming 

| Multi-threading                                                                                                                                | Asynchronous programming                                                                                        |
|------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| Multi-threading is a programming concept where a process is divided into two or more sub-processes, which can be implemented at the same time. | Asynchronous programming is a programming concept where a process is executed without blocking the main thread. |
| Multi-threading is used to execute multiple threads at the same time.                                                                          | Asynchronous programming is used to execute multiple tasks without blocking the main thread.                    |    
| Multi-threading is used to improve the performance of the application.                                                                         | Asynchronous programming is used to improve the responsiveness of the application.                              |
| Multi-threading is used to execute multiple tasks concurrently.                                                                                | Asynchronous programming is used to execute multiple tasks without blocking the main thread.                    |


Your misunderstanding is extremely common. Many people are taught that multithreading and asynchrony are the same thing, but they are not.

An analogy usually helps. You are cooking in a restaurant. An order comes in for eggs and toast.

- Synchronous: you cook the eggs, then you cook the toast.
- Asynchronous, single threaded: you start the eggs cooking and set a timer. You start the toast cooking, and set a timer. While they are both cooking, you clean the kitchen. When the timers go off you take the eggs off the heat and the toast out of the toaster and serve them.
- Asynchronous, multithreaded: you hire two more cooks, one to cook eggs and one to cook toast. Now you have the problem of coordinating the cooks so that they do not conflict with each other in the kitchen when sharing resources. And you have to pay them.

Now does it make sense that multithreading is only one kind of asynchrony? Threading is about workers; asynchrony is about tasks. In multithreaded workflows you assign tasks to workers. In asynchronous single-threaded workflows you have a graph of tasks where some tasks depend on the results of others; as each task completes it invokes the code that schedules the next task that can run, given the results of the just-completed task. But you (hopefully) only need one worker to perform all the tasks, not one worker per task.

It will help to realize that many tasks are not processor-bound. For processor-bound tasks it makes sense to hire as many workers (threads) as there are processors, assign one task to each worker, assign one processor to each worker, and have each processor do the job of nothing else but computing the result as quickly as possible. But for tasks that are not waiting on a processor, you don't need to assign a worker at all. You just wait for the message to arrive that the result is available and do something else while you're waiting. When that message arrives then you can schedule the continuation of the completed task as the next thing on your to-do list to check off.