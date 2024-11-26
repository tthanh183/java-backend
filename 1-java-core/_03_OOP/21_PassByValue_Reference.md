### Pass by value and Pass by reference in Java

In Java, pass-by-value and pass-by-reference are key concepts related to how variables are passed to methods:
- Pass-by-value: When a primitive data type is passed to a method, a copy of the variable's value is passed. This means changes made to the parameter inside the method do not affect the original variable.
- Pass-by-reference: When an object is passed to a method, the reference (memory address) to the object is passed. This means changes made to the object inside the method affect the original object, but the reference itself is still passed by value (you can't change the object reference itself).

Explanation:

In Java, memory is managed using two primary areas: Stack and Heap. Here's how they work:

Stack memory: This is where local variables (including primitive data types) and method calls are stored. When a method is called, a new stack frame is created to store its local variables. Once the method execution is completed, the stack frame is removed. When a primitive type is passed to a method, a copy of the value is pushed onto the stack.

Heap memory: This is where objects are stored. When an object is created, it is allocated space in the heap, and a reference (memory address) to that object is stored in the stack. This reference is used to access the object in the heap.

When a primitive is passed to a method, a copy of the value is passed (call by value), so changes to the parameter inside the method do not affect the original value.

When an object is passed to a method, the reference (memory address) to the object in the heap is passed. This means the method has access to the same object, and changes made to the object will affect the original object in the heap, but the reference itself is passed by value (meaning you can't change the reference itself).

Example:
```java
public class ObjectReferenceExample {
    int data = 50;
    
    void change(int data) {
        data = data + 100; // changes will be in the local variable only
    }
    public static void main(String[] args) {    
        ObjectReferenceExample obj = new ObjectReferenceExample();
        System.out.println("Before change: " + obj.data);
        obj.change(500);
        System.out.println("After change: " + obj.data);
    }
}
```
Output:
```
Before change: 50
After change: 50
```
Example:
```java
public class ObjectReferenceExample {
    int data = 50;
    
    void change(ObjectReferenceExample obj) {
        obj.data = obj.data + 100; // changes will be in the original object
    }
    public static void main(String[] args) {    
        ObjectReferenceExample obj = new ObjectReferenceExample();
        System.out.println("Before change: " + obj.data);
        obj.change(obj);
        System.out.println("After change: " + obj.data);
    }
}
```
Output:
```
Before change: 50
After change: 150
```

