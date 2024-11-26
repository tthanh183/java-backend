### `Final` keyword in Java
The final keyword in Java is used to restrict users. It can be applied in various contexts:
- Final variables: You cannot change the value of a final variable (it becomes a constant).
- Final methods: You cannot override a final method.
- Final classes: You cannot inherit a final class.
- Blank final variables: A final variable that is not initialized at the time of declaration is known as a blank final variable.
> [!NOTE]
> Blank final variables can be initialized in the constructor only.  
> A blank final variable can be static also,in which case it can only be initialized in a static block. 

1. **Final Variables**

If you declare any variable as final, you cannot change the value of the final variable (it will become a constant).
Example:
```java
class Bike {
    final int speedlimit = 90; // final variable
    void run() {
        speedlimit = 400; // will give an error
    }
}
```
Output:
```
error: cannot assign a value to final variable speedlimit
        speedlimit = 400; // will give an error
```
2. **Final Methods**

If you make any method as final, you cannot override it.
Example:
```java
class Bike {
    final void run() {
        System.out.println("running");
    }
}

class Honda extends Bike {
    void run() {
        System.out.println("running safely with 100kmph");
    }

    public static void main(String args[]) {
        Honda honda = new Honda();
        honda.run();
    }
}
```
Output:
```
error: run() in Honda cannot override run() in Bike
    void run() {
         ^
  overridden method is final
```
3. **Final Classes**

If you make any class as final, you cannot inherit it.
Example:
```java
final class Bike {
    void run() {
        System.out.println("running");
    }
}

class Honda extends Bike {
    void run() {
        System.out.println("running safely with 100kmph");
    }

    public static void main(String args[]) {
        Honda honda = new Honda();
        honda.run();
    }
}
```
Output:
```
error: cannot inherit from final Bike
class Honda extends Bike {
                   ^
```
4. **Blank Final Variable**

A final variable that is not initialized at the time of declaration is known as a blank final variable. We must initialize the blank final variable in the constructor of the class.
Example:
```java
class Bike {
    final int speedlimit; // blank final variable
    Bike() {
        speedlimit = 70;
        System.out.println(speedlimit);
    }

    public static void main(String args[]) {
        new Bike();
    }
}
```
Output:
```
70
```
> [!TIP]
> If you want to create a variable that is initialized when the object is created and once it is initialized, it cannot be changed, then a blank final variable is useful in such cases (e.g., employee's PAN card number).
5. **Static Blank Final Variable**
 
A static final variable that is not initialized at the time of declaration is called a static blank final variable. It can only be initialized in a static block at the time of class loading.
Example:
```java
class Bike {
    static final int speedlimit; // static blank final variable
    static {
        speedlimit = 70;
        System.out.println(speedlimit);
    }

    public static void main(String args[]) {
    }
}
```
Output:
```
70
```

### Frequently Asked Questions
Question: Can a final method be inherited?    
Answer: Yes, but you cannot override it.  
Question: What is final parameter?  
Answer: If you declare any parameter as final, you cannot change the value of it.  
Question: Can we declare a constructor as final?  
Answer: No, because the constructor is never inherited.  

























