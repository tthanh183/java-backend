### Method 
A method is a group of statements that perform a specific task. The purpose is to reuse code.
```
access-modifier static/non-static returnType methodName(list[] parameters) {
    // body of method
}
```
- access-modifier: Specifies the accessibility of the code.
- static: Defines the property of the method (it can be called via an object or the class name).
- return type: The type of value the method returns, which can be:
  - void: If there is no return value.
  - a type: If the method returns a primitive type, object, etc.
> [!NOTE]
> If there is a return type, the return keyword must be used, and the return type must match the method's return type.
- Method name: Should be a verb, noun, and follow camelCase rules.
- parameter list: Can have zero or more parameters.