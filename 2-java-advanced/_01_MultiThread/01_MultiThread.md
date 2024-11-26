### MultiThread 

Multithreading in Java refers to the process of executing multiple threads concurrently.

A thread is essentially a subprocess. It is the smallest unit of a process. Both multiprocessing and multithreading are used to create multitasking systems.

However, multithreading is preferred over multiprocessing because threads share a common memory area. They do not allocate separate memory areas, saving memory space, and context switching between threads takes less time than between processes.

Advantages of Multithreading in Java  
- It does not block the user since threads are independent, allowing multiple tasks to be performed simultaneously.
- You can perform several operations concurrently, saving time.
- Threads are independent, so if an exception occurs in one thread, it does not affect other threads.

Multitasking

Multitasking refers to the process of executing multiple tasks simultaneously. We use multitasking to take advantage of the CPU's capabilities. Multitasking can be achieved in two ways:

- Process-based multitasking - Multiprocessing
- Thread-based multitasking - Multithreading

Process-based Multitasking (Multiprocessing)

- Each process has its own memory address, meaning each process allocates its own separate memory.
- Processes are heavier.
- Communication between processes incurs high costs.
- Switching between processes requires time to save and load memory maps, update lists, etc.

Thread-based Multitasking (Multithreading)
- Threads share the same memory space.
- Threads are lightweight.
- Communication between threads is inexpensive.
Note: At least one processor is required for each thread.