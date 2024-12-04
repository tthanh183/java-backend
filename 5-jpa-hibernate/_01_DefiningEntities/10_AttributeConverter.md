### Attribute Converter

1. **Introduction**:

In this quick article, we’ll cover the usage of the Attribute Converters available in JPA 3.0 – which, simply put, allow us to map JDBC types to Java classes.

We’ll use Hibernate 6 as our JPA implementation here.

2. **Creating a Converter**:

We’re going to show how to implement an attribute converter for a custom Java class.

First, let’s create a PersonName class – that will be converted later:

```java
public class PersonName implements Serializable {

    private String name;
    private String surname;

    // getters and setters
}   
```

Then, we’ll add an attribute of type PersonName to an @Entity class:

```java
@Entity(name = "PersonTable")
public class Person {
   
    private PersonName personName;

    //...
}
```
Now we need to create a converter that transforms the PersonName attribute to a database column and vice-versa. In our case, we’ll convert the attribute to a String value that contains both name and surname fields.

To do so we have to annotate our converter class with @Converter and implement the AttributeConverter interface. We’ll parametrize the interface with the types of the class and the database column, in that order:

```java
@Converter
public class PersonNameConverter implements 
  AttributeConverter<PersonName, String> {

    private static final String SEPARATOR = ", ";

    @Override
    public String convertToDatabaseColumn(PersonName personName) {
        if (personName == null) {
            return null;
        }

        StringBuilder sb = new StringBuilder();
        if (personName.getSurname() != null && !personName.getSurname()
            .isEmpty()) {
            sb.append(personName.getSurname());
            sb.append(SEPARATOR);
        }

        if (personName.getName() != null 
          && !personName.getName().isEmpty()) {
            sb.append(personName.getName());
        }

        return sb.toString();
    }

    @Override
    public PersonName convertToEntityAttribute(String dbPersonName) {
        if (dbPersonName == null || dbPersonName.isEmpty()) {
            return null;
        }

        String[] pieces = dbPersonName.split(SEPARATOR);

        if (pieces == null || pieces.length == 0) {
            return null;
        }

        PersonName personName = new PersonName();        
        String firstPiece = !pieces[0].isEmpty() ? pieces[0] : null;
        if (dbPersonName.contains(SEPARATOR)) {
            personName.setSurname(firstPiece);

            if (pieces.length >= 2 && pieces[1] != null 
              && !pieces[1].isEmpty()) {
                personName.setName(pieces[1]);
            }
        } else {
            personName.setName(firstPiece);
        }

        return personName;
    }
}
```

Notice that we had to implement 2 methods: convertToDatabaseColumn() and convertToEntityAttribute().

The two methods are used to convert from the attribute to a database column and vice-versa.

3. **Using the Converter**:

To use our converter, we just need to add the @Convert annotation to the attribute and specify the converter class we want to use:

```java
@Entity(name = "PersonTable")
public class Person {

    @Convert(converter = PersonNameConverter.class)
    private PersonName personName;
    
    // ...
}
```

Finally, let’s create a unit test to see that it really works.

To do so, we’ll first store a Person object in our database:

```java
@Test
public void givenPersonName_whenSaving_thenNameAndSurnameConcat() {
    String name = "name";
    String surname = "surname";

    PersonName personName = new PersonName();
    personName.setName(name);
    personName.setSurname(surname);

    Person person = new Person();
    person.setPersonName(personName);

    Long id = (Long) session.save(person);

    session.flush();
    session.clear();
}
```

Next, we’re going to test that the PersonName was stored as we defined it in the converter – by retrieving that field from the database table:

```java
@Test
public void givenPersonName_whenSaving_thenNameAndSurnameConcat() {
    // ...

    String dbPersonName = (String) session.createNativeQuery(
      "select p.personName from PersonTable p where p.id = :id")
      .setParameter("id", id)
      .getSingleResult();

    assertEquals(surname + ", " + name, dbPersonName);
}
```

Let’s also test that the conversion from the value stored in the database to the PersonName class works as defined in the converter by writing a query that retrieves the whole Person class:

```java
@Test
public void givenPersonName_whenSaving_thenNameAndSurnameConcat() {
    // ...

    Person dbPerson = session.createNativeQuery(
      "select * from PersonTable p where p.id = :id", Person.class)
        .setParameter("id", id)
        .getSingleResult();

    assertEquals(dbPerson.getPersonName()
      .getName(), name);
    assertEquals(dbPerson.getPersonName()
      .getSurname(), surname);
}
```