### Operators in Java
An operator in Java is a symbol used to perform a specific operation or function. Java provides several types of operators:  
- Arithmetic Operators
- Bitwise Operators
- Relational Operators
- Logical Operators
- Conditional Operators
- Assignment Operators

1. **Arithmetic Operators**

Arithmetic operators work with numeric operands (excluding boolean). Character operands can also be used with these operators.
Below is a summary of the common arithmetic operators:
Assume we have the integer variables a = 10 and b = 20.

| Operator | Description         | Example              |
|----------|---------------------|----------------------|
| `+`      | Addition            | a + b results in 30  |
| `-`      | Subtraction         | a - b results in -10 |
| `*`      | Multiplication      | a * b results in 200 |
| `/`      | Division            | b / a results in 2   |
| `%`      | Modulus             | b % a results in 0   |
| `++`     | Increment           | a++ results in 11    |
| `--`     | Decrement           | a-- results in 9     |
| `+=`     | Add and assign      | a += 2 results in 12 |
| `-=`     | Subtract and assign | a -= 2 results in 8  |
| `*=`     | Multiply and assign | a *= 2 results in 20 |
| `/=`     | Divide and assign   | a /= 2 results in 5  |
| `%=`     | Modulus and assign  | a %= 8 results in 4  |


2. **Bitwise Operators**

Bitwise operators allow us to manipulate individual bits in primitive data types.

| Operator | Description                                                                                                              |
|----------|--------------------------------------------------------------------------------------------------------------------------|
| `~`      | Bitwise NOT  Returns the negation of a bit.                                                                              |
| `&`      | Bitwise AND  Returns `1` if both operands are `1`; otherwise, returns `0`.                                               |
| `\|`     | Bitwise OR  Returns `1` if at least one operand is `1`; otherwise, returns `0`.                                          |
| `^`      | Bitwise XOR (Exclusive OR)  Returns `1` if only one operand is `1`; otherwise, returns `0`.                              |
| `>>`     | Right Shift  Shifts all bits of a number to the right by the specified positions, keeping the sign of a negative number. |
| `<<`     | Left Shift  Shifts all bits of a number to the left by the specified positions, keeping the sign of a negative number.   |

4. **Relational Operators**

Relational operators are used to check the relationship between two operands. The result of an expression using relational operators is a Boolean value (`true` or `false`). These operators are often used in control structures.

| Operator | Description                                                                                      |
|----------|--------------------------------------------------------------------------------------------------|
| `==`     | Equality  Checks if two operands are equivalent.                                                 |
| `!=`     | Not Equal  Checks if two operands are different.                                                 |
| `>`      | Greater Than  Checks if the left operand is greater than the right operand.                      |
| `<`      | Less Than  Checks if the left operand is less than the right operand.                            |
| `>=`     | Greater Than or Equal  Checks if the left operand is greater than or equal to the right operand. |
| `<=`     | Less Than or Equal  Checks if the left operand is less than or equal to the right operand.       |

5. **Logical Operators**

Logical operators work with Boolean operands. They are commonly used in control structures.

| Operator | Description                                                                                                  |
|----------|--------------------------------------------------------------------------------------------------------------|
| `&&`     | Logical AND  Returns `true` only if both operands are `true`.                                                |
| `\| \|`  | Logical OR  Returns `true` if at least one operand is `true`.                                                |                             |
| `^`      | Logical XOR (Exclusive OR)   Returns `true` if and only if one operand is `true`, otherwise returns `false`. |
| `!`      | Logical NOT  A unary operator that flips the operand's value: `true` becomes `false`, and vice versa         |

6. **Conditional Operator**

The conditional operator is a special operator consisting of three components that form a conditional expression. Syntax:
```
<condition> ? <expression1> : <expression2>;
```
7. **Assignment Operator**

The assignment operator (=) is used to assign a value to a variable. It can also assign the same value to multiple variables in one statement.
```
int var = 20;
int p, q, r, s;
p = q = r = s = var;
```
8. **Operator Precedence**

Operator precedence determines the order in which operators are evaluated in expressions. The table below lists the operator precedence in Java:

| Precedence | Description                                               |
|------------|-----------------------------------------------------------|
| 1          | Unary operators such as +, -, ++, --                      |
| 2          | Arithmetic and shift operators such as *, /, +, -, <<, >> |                             
| 3          | Relational operators such as >, <, >=, <=, ==, !=         |
| 4          | Logical and bitwise operators such as &&, `               |
| 5          | Assignment operators such as =, *=, /=, +=, -=            |

9. **Changing Operator Precedence**

To alter the precedence in an expression, you can use parentheses (()):

- The portion enclosed in parentheses is evaluated first.
- For nested parentheses, the innermost parentheses are evaluated first, followed by the outer ones.
- Inside a single set of parentheses, the default precedence rules still apply.

Example:
```java
public class Test {
    public static void main(String[] args) {
        int a = 20;
        int b = 5;
        int c = 10;
 
        System.out.println("a + b * c   = " + (a + b * c));
        System.out.println("(a + b) * c = " + ((a + b) * c));
        System.out.println("a / b - c   = " + (a / b - c));
        System.out.println("a / (b - c) = " + (a / (b - c)));
    }
}
```
Output:
```
a + b * c   = 70
(a + b) * c = 250
a / b - c   = -6
a / (b - c) = -4
```