### Mapping Single Entity to Multiple Tables

1. **Overview**:

JPA makes dealing with relational database models from our Java applications less painful. Things are simple when we map every table to a single entity class.

But we sometimes have reasons to model our entities and tables differently:

- When we want to create logical groups of fields, we can map multiple classes to a single table.
- If inheritance is involved, we can map a class hierarchy to a table structure.
- In cases when related fields are scattered between multiple tables and we want to model those tables with a single class  

In this short tutorial, we’ll see how to tackle this last scenario.

2. **Data Model**:

Let’s say we run a restaurant, and we want to store data about every meal we serve:

- Name
- Description
- Price
- What kind of allergens it contains
Since there are many possible allergens, we’re going to group this data set together.

Furthermore, we’ll also model this using the following table definitions:

![Mapping Single Entity to Multiple Tables](https://www.baeldung.com/wp-content/uploads/2019/10/meals.png)

Now let’s see how we can map these tables to entities using standard JPA annotations.

3. **Creating Multiple Entities**:

The most obvious solution is to create an entity for both classes.

Let’s start by defining the MealWithMultipleEntities entity:

```java
@Entity
@Table(name = "meal")
public class MealWithMultipleEntities {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    Long id;

    @Column(name = "name")
    String name;

    @Column(name = "description")
    String description;

    @Column(name = "price")
    BigDecimal price;

    @OneToOne(mappedBy = "meal")
    AllergensAsEntity allergens;

    // standard getters and setters
}
```

Next, we’ll add the AllergensAsEntity entity:

```java
@Entity
@Table(name = "allergens")
class AllergensAsEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "meal_id")
    Long mealId;

    @OneToOne
    @PrimaryKeyJoinColumn(name = "meal_id")
    Meal meal;

    @Column(name = "peanuts")
    boolean peanuts;

    @Column(name = "celery")
    boolean celery;

    @Column(name = "sesame_seeds")
    boolean sesameSeeds;

    // standard getters and setters
}
```

We can see that meal_id is both the primary key and also the foreign key. That means we need to define the one-to-one relationship column using @PrimaryKeyJoinColumn.

However, this solution has two problems:  
    - We always want to store allergens for a meal, and this solution doesn’t enforce this rule.  
    - The meal and allergen data belong together logically. Therefore, we might want to store this information in the same Java class even though we created multiple tables for them.  

One possible resolution to the first problem is to add the @NotNull annotation to the allergens field on our MealWithMultipleEntities entity. JPA won’t let us persist the MealWithMultipleEntities if we have a null AllergensAsEntity.

However, this is not an ideal solution. We want a more restrictive one, where we don’t even have the opportunity to try to persist a MealWithMultipleEntities without AllergensAsEntity.

4. **Creating a Single Entity With @SecondaryTable**:

We can create a single entity specifying that we have columns in different tables using the @SecondaryTable annotation:

```java

@Entity
@Table(name = "meal")
@SecondaryTable(name = "allergens", pkJoinColumns = @PrimaryKeyJoinColumn(name = "meal_id"))
class MealAsSingleEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    Long id;

    @Column(name = "name")
    String name;

    @Column(name = "description")
    String description;

    @Column(name = "price")
    BigDecimal price;

    @Column(name = "peanuts", table = "allergens")
    boolean peanuts;

    @Column(name = "celery", table = "allergens")
    boolean celery;

    @Column(name = "sesame_seeds", table = "allergens")
    boolean sesameSeeds;

    // standard getters and setters

}
```

Behind the scenes, JPA joins the primary table with the secondary table and populates the fields. This solution is similar to the @OneToOne relationship, but this way, we can have all of the properties in the same class.

It’s important to note that if we have a column that is in a secondary table, we have to specify it with the table argument of the @Column annotation. If a column is in the primary table, we can omit the table argument since JPA looks for columns in the primary table by default.

Also, note that we can have multiple secondary tables if we embed them in @SecondaryTables. Alternatively, from Java 8, we can mark the entity with multiple @SecondaryTable annotations since it’s a repeatable annotation.

5. **Combining @SecondaryTable With @Embedded**:

As we’ve seen, @SecondaryTable maps multiple tables to the same entity. We also know that @Embedded and @Embeddable do the opposite and map a single table to multiple classes.

Let’s see what we get when we combine @SecondaryTable with @Embedded and @Embeddable:

```java
@Entity
@Table(name = "meal")
@SecondaryTable(name = "allergens", pkJoinColumns = @PrimaryKeyJoinColumn(name = "meal_id"))
class MealWithEmbeddedAllergens {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    Long id;

    @Column(name = "name")
    String name;

    @Column(name = "description")
    String description;

    @Column(name = "price")
    BigDecimal price;

    @Embedded
    AllergensAsEmbeddable allergens;

    // standard getters and setters

}

@Embeddable
class AllergensAsEmbeddable {

    @Column(name = "peanuts", table = "allergens")
    boolean peanuts;

    @Column(name = "celery", table = "allergens")
    boolean celery;

    @Column(name = "sesame_seeds", table = "allergens")
    boolean sesameSeeds;

    // standard getters and setters

}
```

It’s a similar approach to what we saw using @OneToOne. However, it has a couple of advantages:
- JPA manages the two tables together for us, so we can be sure that there will be a row for each meal in both tables.
- Also, the code is a bit simpler since we need less configuration.

Nevertheless, this one-to-one-like solution works only when the two tables have matching ids.

It’s worth mentioning that if we want to reuse the AllergensAsEmbeddable class, it would be better if we defined the columns of the secondary table in the MealWithEmbeddedAllergens class with @AttributeOverride.




