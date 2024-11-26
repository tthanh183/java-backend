### CompletableFuture in Java 8

CompletableFuture is a feature in Java 8's java.util.concurrent package designed for asynchronous programming. It extends the Future interface and introduces powerful methods to compose tasks, handle results, and deal with errors effectively.

> [!INFO]  
> `CompletableFuture` is used for asynchronous programming in Java. It is a class in Java that implements the `Future` interface and provides a way to write non-blocking, asynchronous code. It is used to perform tasks asynchronously and return the result to the calling thread.  
> You can use `MultiThread` for multi-threading and `CompletableFuture` for asynchronous programming for each thread. 

1. **Creating CompletableFuture**

There are several ways to create a `CompletableFuture` object:
- Using the `supplyAsync()` method
```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    return "Hello, World!";
});
```
- Using the `runAsync()` method (for tasks that don't return a value)
```java
CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
    System.out.println("Hello, World!");
});
```
- Using the `CompletableFuture` constructor
```java
CompletableFuture<String> future = new CompletableFuture<>();
future.complete("Hello, World!");
```

> [!NOTE]  
> CompletableFuture<T>: s a generic class in Java, meaning it can hold a value of any type T that you specify. In this context: T is the type of the value that the CompletableFuture will hold. You can think of it as the return type of the asynchronous task.
2. **CompletableFuture Methods**

- `thenApply()` method: Applies a function to the result of the `CompletableFuture` when it completes.
```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    return "Hello, World!";
});
CompletableFuture<String> future2 = future.thenApply(result -> {
    return "Hello, " + result;
});
```
- `thenAccept()` method: Accepts the result of the `CompletableFuture` when it completes.
```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    return "Hello, World!";
});
CompletableFuture<Void> future2 = future.thenAccept(result -> {
    System.out.println("Received: " + result);
});
```
- `thenRun()` method: Runs a `Runnable` when the `CompletableFuture` completes.
```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    return "Hello, World!";
});
CompletableFuture<Void> future2 = future.thenRun(() -> {
    System.out.println("Done!");
});
```
- `thenCompose()` method: flatMap for `CompletableFuture`.
```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    return "Hello, World!";
});
CompletableFuture<String> future2 = future.thenCompose(result -> {
    return CompletableFuture.supplyAsync(() -> {
        return "Hello, " + result;
    });
});
```
- `thenCombine()` method: Combines results of two `CompletableFuture` objects when both complete.
```java
CompletableFuture<String> future1 = CompletableFuture.supplyAsync(() -> {
    return "Hello";
});
CompletableFuture<String> future2 = CompletableFuture.supplyAsync(() -> {
    return "World!";
});
CompletableFuture<String> future3 = future1.thenCombine(future2, (result1, result2) -> {
    return result1 + " " + result2;
});
```

3. **Exception Handling**

- `exceptionally()` method: Handles exceptions when they occur.
```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    throw new RuntimeException("Exception occurred!");
});
CompletableFuture<String> future2 = future.exceptionally(ex -> {
    return "Hello, World!";
});
```
- `handle()` method: Handles both the result and the exception.
```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    throw new RuntimeException("Exception occurred!");
});
CompletableFuture<String> future2 = future.handle((result, ex) -> {
    if (ex != null) {
        return "Hello, World!";
    }
    return result;
});
```

4. **Combining CompletableFutures**

- `allOf()` method: Combines multiple `CompletableFuture` objects and returns a new `CompletableFuture` that completes when all of the given `CompletableFuture` objects complete.
```java
CompletableFuture<String> future1 = CompletableFuture.supplyAsync(() -> {
    return "Hello";
});
CompletableFuture<String> future2 = CompletableFuture.supplyAsync(() -> {
    return "World!";
});
CompletableFuture<Void> combinedFuture = CompletableFuture.allOf(future1, future2);
```
- `anyOf()` method: Combines multiple `CompletableFuture` objects and returns a new `CompletableFuture` that completes when any of the given `CompletableFuture` objects complete.
```java
CompletableFuture<String> future1 = CompletableFuture.supplyAsync(() -> {
    return "Hello";
});
CompletableFuture<String> future2 = CompletableFuture.supplyAsync(() -> {
    return "World!";
});
CompletableFuture<Object> anyOfFuture = CompletableFuture.anyOf(future1, future2);
```
