### Object class in Java

By default, the `Object` class is the superclass of all classes in Java. In other words, it is the topmost class in the Java class hierarchy.

Using the `Object` class is useful when you want to reference any object whose type is not known.

> [!Note]   
> a reference variable of the superclass can refer to an object of the subclass, which is known as upcasting.

The Object class provides several common functionalities for all objects, such as allowing objects to be compared, cloned, and notified.

| Method                                            | Description                                                                                                                                                                                                                       |
|---------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `public final Class getClass()`                   | Returns the runtime class of an object.                                                                                                                                                                                           |
| `public int hashCode()`                           | Returns the hash code value for the object.                                                                                                                                                                                       |
| `public boolean equals(Object obj)`               | Indicates whether some other object is "equal to" this one.                                                                                                                                                                       |
| `protected Object clone()`                        | Creates and returns a copy of this object.                                                                                                                                                                                        |
| `public String toString()`                        | Returns a string representation of the object.                                                                                                                                                                                    |
| `public final void notify()`                      | Wakes up a single thread that is waiting on this object's monitor.                                                                                                                                                                |
| `public final void notifyAll()`                   | Wakes up all threads that are waiting on this object's monitor.                                                                                                                                                                   |
| `public final void wait(long timeout)`            | Causes the current thread to wait until another thread calls the `notify()` method or the `notifyAll()` method for this object.                                                                                                   |
| `public final void wait(long timeout, int nanos)` | Causes the current thread to wait until another thread calls the `notify()` method or the `notifyAll()` method for this object, or some other thread interrupts the current thread, or a certain amount of real time has elapsed. |
| `public final void wait()`                        | Causes the current thread to wait until another thread calls the `notify()` method or the `notifyAll()` method for this object.                                                                                                   |
| `protected void finalize()`                       | Called by the garbage collector on an object when garbage collection determines that there are no more references to the object.                                                                                                  |
