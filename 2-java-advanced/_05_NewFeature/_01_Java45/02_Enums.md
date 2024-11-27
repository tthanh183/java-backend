### Enums 

`Enum` in Java is a special data type used to define a set of constants. Specifically, an `enum` in Java is a special type of class. An `enum` can contain fields, methods, and constructors.

`Enums` can be used to define values like the days of the week (SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY) or the seasons of the year (SPRING, SUMMER, FALL, WINTER), etc.

`Enums` were introduced in Java in version 5, along with other features.

1. **Enum Declaration**:
Enums are declared using the `enum` keyword. 

- Enums is declared inside a class or interface.

Example:
```java
public class EnumExample {
    enum Days {
        SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
    }
    public static void main(String[] args) {
        Days day = Days.MONDAY;
        System.out.println(day);
    }
}
```
Output:
```
MONDAY
```

- Enums is declared outside a class or interface.

Example:
```java
enum Days {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
}
public class EnumExample {
    public static void main(String[] args) {
        Days day = Days.MONDAY;
        System.out.println(day);
    }
}
```
Output:
```
MONDAY
```

- Enums is declared in a separate file.

Example:
```java
// Days.java
enum Days {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
}

// EnumExample.java
public class EnumExample {
    public static void main(String[] args) {
        Days day = Days.MONDAY;
        System.out.println(day);
    }
}
```
Output:
```
MONDAY
```

2. **Iterating through enum elements**:

The Java compiler automatically adds some special methods when it creates an enum. One of these methods is the `values()` method, which returns an array of all the enum constants.

Example:
```java
enum Days {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
}
public class EnumExample {
    public static void main(String[] args) {
        Days[] days = Days.values();
        for (Days day : days) {
            System.out.println(day);
        }
    }
}
```
Output:
```
SUNDAY
MONDAY
TUESDAY
WEDNESDAY
THURSDAY
FRIDAY
SATURDAY
```

3. **Initializing special values for enum constants**:

Enums constants typically  represent fixed values like: 0, 1, 2, etc. However, you can also assign special values to enum constants.

Example:
```java
enum Days {
    SUNDAY(1), MONDAY(2), TUESDAY(3), WEDNESDAY(4), THURSDAY(5), FRIDAY(6), SATURDAY(7);
    private int value;
    private Days(int value) {
        this.value = value;
    }
    public int getValue() {
        return value;
    }
}
public class EnumExample {
    public static void main(String[] args) {
        Days day = Days.MONDAY;
        System.out.println(day.getValue());
    }
}
```
Output:
```
2
```

You can also define many values for each enum constant.

Example:
```java

enum Days {
    SUNDAY(1, "Sunday"), MONDAY(2, "Monday"), TUESDAY(3, "Tuesday"), WEDNESDAY(4, "Wednesday"), THURSDAY(5, "Thursday"), FRIDAY(6, "Friday"), SATURDAY(7, "Saturday");
    private int value;
    private String name;
    private Days(int value, String name) {
        this.value = value;
        this.name = name;
    }
    public int getValue() {
        return value;
    }
    public String getName() {
        return name;
    }
}

public class EnumExample {
    public static void main(String[] args) {
        Days day = Days.MONDAY;
        System.out.println(day.getValue());
        System.out.println(day.getName());
    }
}
```

> [!NOTE]  
> Constructors in enums are always private. The JVM will automatically create the private constructor for the enum constants.

4. **Using Java enum in switch statements**:

Enums can be used in switch statements.

Example:
```java
enum Days {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
}

public class EnumExample {
    public static void main(String[] args) {
        Days day = Days.MONDAY;
        switch (day) {
            case SUNDAY:
                System.out.println("Sunday");
                break;
            case MONDAY:
                System.out.println("Monday");
                break;
            case TUESDAY:
                System.out.println("Tuesday");
                break;
            case WEDNESDAY:
                System.out.println("Wednesday");
                break;
            case THURSDAY:
                System.out.println("Thursday");
                break;
            case FRIDAY:
                System.out.println("Friday");
                break;
            case SATURDAY:
                System.out.println("Saturday");
                break;
        }
    }
}
```
Output:
```
Monday
```

5. **Comparing enum elements**:

You can compare enum elements using the `==` operator or the `equals()` method.

Example:
```java

enum Days {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
}

public class EnumExample {
    public static void main(String[] args) {
        Days day1 = Days.MONDAY;
        Days day2 = Days.MONDAY;
        System.out.println(day1 == day2);
        System.out.println(day1.equals(day2));
    }
}
```
Output:
```
true
true
```

6. **Questions about enums in Java**:

- Can an enum be declared as an abstract class?
- Can an enum be declared as an interface?
-> Yes, an enum can be declared as an abstract class or an interface.

- Can we create an object of an enum?
-> No, you cannot create an object of an enum using the `new` keyword.