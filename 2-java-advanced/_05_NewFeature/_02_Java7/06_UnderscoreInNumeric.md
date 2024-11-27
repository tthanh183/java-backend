### Underscore in Numeric Literals

Java allows you to use underscores in numeric literals, a feature introduced in Java 7. This feature enables you to separate groups of digits in large numbers, making the code more readable.

Restrictions for using underscores in numeric literals:

- You cannot place an underscore at the beginning or end of a number.
```java
int x = _100; // not allowed
int y = 100_; // not allowed
```
- You cannot place an underscore adjacent to a decimal point.
```java
double z = 100_.00; // not allowed
```
- You cannot place an underscore adjacent to an 'L' or 'l' suffix.
```java
long l = 100_L; // not allowed
```

Example:
```java

public class UnderscoreInNumeric {
    public static void main(String[] args) {
        int million = 1_000_000;
        int billion = 1_000_000_000;
        long creditCardNumber = 1234_5678_9012_3456L;
        long socialSecurityNumber = 999_99_9999L;
        float pi = 3.14_15F;
        long hexBytes = 0xFF_EC_DE_5E;
        long hexWords = 0xCAFE_BABE;
        long maxLong = 0x7fff_ffff_ffff_ffffL;
        byte nybbles = 0b0010_0101;
        long bytes = 0b11010010_01101001_10010100_10010010;
    }
}
```
