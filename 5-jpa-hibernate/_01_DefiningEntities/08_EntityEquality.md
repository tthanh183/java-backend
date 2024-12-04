### Entity Equality

1. **Overview**:

In this tutorial, we’ll take a look at handling equality with JPA Entity objects.

2. **Considerations**:

In general, equality simply means that two objects are the same. However, in Java, we can change the definition of equality by overriding the Object.equals() and the Object.hashCode() methods. Ultimately, Java allows us to define what it means to be equal. But first, there are a few things we need to consider.

- Collections

Java collections group objects together. The grouping logic uses a special value known as a hash code to determine the group for an object.

If the value returned by the hashCode() method is the same for all entities, this could result in undesired behavior. Let’s say our entity object has a primary key defined as id, but we define our hashCode() method as:

```java
@Override
public int hashCode() {
    return 12345;
}
```

Collections will not be able to distinguish between different objects when comparing them because they will all share the same hash code. Luckily, resolving this is as easy as using a unique key when generating a hash code. For example, we can define the hashCode() method using our id:

```java
@Override
public int hashCode() {
    return id * 12345;
}
```

In this case, we used the id of our entity to define the hash code. Now, collections can compare, sort, and store our entities.

- Transient Entities

Newly created JPA entity objects that have no association with a persistence context are considered to be in the transient state. These objects usually do not have their @Id members populated. Therefore, if equals() or hashCode() use the id in their calculations, this means all transient objects will be equal because their ids will all be null. There are not many cases where this is desirable.

- Subclasses

Subclasses are also a concern when defining equality. It’s common to compare classes in the equals() method. Therefore, including the getClass() method will help to filter out subclasses when comparing objects for equality.

Let’s define an equals() method that will only work if the objects are of the same class and have the same id:

```java
@Override
public boolean equals(Object o) {
    if (o == null || this.getClass() != o.getClass()) {
        return false;
    }
    return o.id.equals(this.id);
}
```

3. **Defining Equality**:

- No Overriding
By default, Java provides the equals() and hashCode() methods by virtue of all objects descending from the Object class. Therefore, the easiest thing we can do is nothing. Unfortunately, this means that when comparing objects, in order to be considered equal, they have to be the same instances and not two separate instances representing the same object.

- Using a Database Key

In most cases, we’re dealing with JPA entities that are stored in a database. Normally, these entities have a primary key that is a unique value. Therefore, any instances of this entity that have the same primary key value are equal. So, we can override equals() as we did above for subclasses and also override hashCode() using only the primary key in both.

- Using a Business Key

Alternatively, we can use a business key to compare JPA entities. In this case, the object’s key is comprised of members of the entity other than the primary key. This key should make the JPA entity unique. Using a business key gives us the same desired outcome when comparing entities without the need for primary or database-generated keys.

Let’s say we know that an email address is always going to be unique, even if it isn’t the @Id field. We can include the email field in hashCode() and equals() methods:

```java

public class EqualByBusinessKey {

    private String email;

    @Override
    public int hashCode() {
        return java.util.Objects.hashCode(email);
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }
        if (obj == null) {
            return false;
        }
        if (obj instanceof EqualByBusinessKey) {
            if (((EqualByBusinessKey) obj).getEmail().equals(getEmail())) {
                return true;
            }
        }

        return false;
    }
}
```

