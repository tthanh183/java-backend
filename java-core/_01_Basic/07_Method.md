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
    > [!WARNING]
    > If there is a return type, the method must use the return keyword, and the type of the return value must `match` the type specified at the beginning of the method.
- Method name: Should be a verb, noun, and follow camelCase rules.
- parameter list: Can have zero or more parameters.