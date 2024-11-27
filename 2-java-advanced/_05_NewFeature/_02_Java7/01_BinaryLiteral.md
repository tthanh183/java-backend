### Binary Literal

Java introduced a new feature for binary literals in Java 7. You can now represent integer types (byte, short, int, and long) in binary format. To specify a binary literal, simply add the prefix 0b or 0B before the binary value.

Example:
```java
public class BinaryLiteralExample {
    public static void main(String[] args) {
        byte b = 0b1010;
        short s = 0B1010;
        int i = 0b1010;
        long l = 0B1010;

        System.out.println("Byte: " + b);
        System.out.println("Short: " + s);
        System.out.println("Int: " + i);
        System.out.println("Long: " + l);
    }
}
```
Output:
```
Byte: 10
Short: 10
Int: 10
Long: 10
```
