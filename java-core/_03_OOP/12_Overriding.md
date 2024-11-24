### Method Overriding in Java

Method overriding in Java occurs when a subclass provides a specific implementation for a method that is already defined in its superclass.
> [!NOTE]
> Method overriding is also known as runtime polymorphism. It is achieved by the JVM based on the object used to call the method.

Principles of Method Overriding in Java
1. The method must have the same name as the method in the superclass.
2. The method must have the same parameters as the method in the superclass.
3. The subclass and superclass must have an inheritance relationship.

[Read this file to see more about overriding](10_Polymorphism.md#polymorphism)

### Frequently Asked Questions
Question: Can we override a static method in Java?  
Answer: No, you cannot override a static method in Java. You can only hide a static method in a subclass by declaring a static method with the same signature in the subclass. This is known as method hiding.
Question: Can we override a private method in Java?  
Answer: No, you cannot override a private method in Java because private methods are not inherited. They are only accessible within the class in which they are defined.
Question: Why can't we override a static method in Java?
Answer: In Java, static methods belong to the class, not the object. They are resolved at compile time, not runtime. Therefore, the concept of overriding does not apply to static methods.
