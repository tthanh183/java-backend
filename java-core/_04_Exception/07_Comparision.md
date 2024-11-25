### Throw and Throws in Java

| `throw`                                                              | `throws`                                                                                                                |
|----------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|
| The throw keyword in Java is used to explicitly throw an exception.  | The throws keyword in Java is used to declare an exception.                                                             |
| A checked exception is not thrown if only the throw keyword is used. | A checked exception is thrown even if only the throws keyword is used.                                                  |
| After throw, there is an instance of the exception.                  | After throws, there is one or more classes of exceptions.                                                               |
| throw is used within a method.                                       | throws is declared right after the closing parenthesis of the method.                                                   |
| You cannot throw multiple exceptions.                                | You can declare multiple exceptions using throws, for example: public void method() throws IOException, SQLException;.` |

### Final, Finally, and Finalize in Java

| `final`                                                                                                                                                                                              | `finally`                                                                                                | `finalize`                                                                                 |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| final is used to apply restrictions to classes, methods, and variables. A final class cannot be inherited, a final method cannot be overridden, and the value of a final variable cannot be changed. | finally is used to execute important code; it is always executed whether or not an exception is handled. | finalize is used to perform cleanup processing just before an object is garbage collected. |
| final is a keyword.                                                                                                                                                                                  | finally is a block.                                                                                      | finalize is a method.                                                                      |
