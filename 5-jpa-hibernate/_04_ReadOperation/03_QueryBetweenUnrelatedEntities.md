### JPA Query between Unrelated Entities

1. **Overview**

In this tutorial, we’ll see how we can construct a JPA query between unrelated entities.

2. **Maven Dependencies**

Let’s start by adding the necessary dependencies to our pom.xml.

Then, we add a dependency for the Hibernate ORM which implements the Java Persistence API:

```xml
<dependency>
    <groupId>org.hibernate.orm</groupId>
    <artifactId>hibernate-core</artifactId>
    <version>6.5.2.Final</version>
</dependency>
```

The jakarta persistence API comes as a transient dependency to the hibernate-core.

And finally, we add some QueryDSL dependencies; namely, querydsl-apt and querydsl-jpa:

```xml
<dependency>
    <groupId>com.querydsl</groupId>
    <artifactId>querydsl-apt</artifactId>
    <classifier>jakarta</classifier>
    <version>5.0.0</version>
    <scope>provided</scope>
</dependency>
<dependency>
    <groupId>com.querydsl</groupId>
    <artifactId>querydsl-jpa</artifactId>
    <classifier>jakarta</classifier>
    <version>5.0.0</version>
</dependency>
```

Adding jakarta.xml.bind-api:

```xml
<dependency>
       <groupId>jakarta.xml.bind</groupId>
       <artifactId>jakarta.xml.bind-api</artifactId>
       <version>4.0.0</version>
</dependency>
```

3. **The Domain Model**

The domain of our example is a cocktail bar. Here we have two tables in the database:
- The menu table to store the cocktails that our bar sells and their prices, and
- The recipes table stores the instructions for creating a cocktail

![JPA Query between Unrelated Entities](https://www.baeldung.com/wp-content/uploads/2020/04/one-to-one-768x177.png)

These two tables are not strictly related to each other. A cocktail can be in our menu without keeping instructions for its recipe. Additionally, we could have available recipes for cocktails that we don’t sell yet.

In our example, we are going to find all the cocktails on our menu that we have an available recipe.

4. **The JPA Entities**

We can easily create two JPA entities to represent our tables:

```java
@Entity
@Table(name = "menu")
public class Cocktail {
    @Id
    @Column(name = "cocktail_name")
    private String name;

    @Column
    private double price;

    // getters & setters
}
```

```java
@Entity
@Table(name="recipes")
public class Recipe {
    @Id
    @Column(name = "cocktail")
    private String cocktail;

    @Column
    private String instructions;
    
    // getters & setters
}
```

Between the menu and recipes tables, there is an underlying one-to-one relationship without an explicit foreign key constraint. For example, if we have a menu record where its cocktail_name column’s value is “Mojito” and a recipes record where its cocktail column’s value is “Mojito”, then the menu record is associated with this recipes record.

To represent this relationship in our Cocktail entity, we add the recipe field annotated with various annotations:

```java
@Entity
@Table(name = "menu")
public class Cocktail {
    // ...
 
    @OneToOne
    @JoinColumn(name = "cocktail_name", 
       referencedColumnName = "cocktail", 
       insertable = false, updatable = false, 
       foreignKey = @jakarta.persistence
         .ForeignKey(value = ConstraintMode.NO_CONSTRAINT))
    private Recipe recipe;
   
    // ...
}
```

The first annotation is @OneToOne, which declares the underlying one-to-one relationship with the Recipe entity.

Next, we annotate the recipe field with the @NotFound(action = NotFoundAction.IGNORE) Hibernate annotation. This tells our ORM to not throw an exception when there is a recipe for a cocktail that doesn’t exist in our menu table.

The annotation that associates the Cocktail with its associated Recipe is @JoinColumn. By using this annotation, we define a pseudo foreign key relationship between the two entities.

Finally, by setting the foreignKey property to @javax.persistence.ForeignKey(value = ConstraintMode.NO_CONSTRAINT), we instruct the JPA provider not to generate the foreign key constraint.

5. **The JPA and QueryDSL Queries**

Since we are interested in retrieving the Cocktail entities that are associated with a Recipe, we can query the Cocktail entity by joining it with its associated Recipe entity.

One way we can construct the query is by using JPQL:

```
entityManager.createQuery("select c from Cocktail c join c.recipe")
```

Or by using the QueryDSL framework:

```
new JPAQuery<Cocktail>(entityManager)
  .from(QCocktail.cocktail)
  .join(QCocktail.cocktail.recipe)
```

Another way to get the desired results is to join the Cocktail with the Recipe entity and by using the on clause to define the underlying relationship in the query directly.

We can do this using JPQL:

```
entityManager.createQuery("select c from Cocktail c join Recipe r on c.name = r.cocktail")
```

or by using the QueryDSL framework:

```
new JPAQuery(entityManager)
  .from(QCocktail.cocktail)
  .join(QRecipe.recipe)
  .on(QCocktail.cocktail.name.eq(QRecipe.recipe.cocktail))
```

6. **One-To-One Join Unit Test**

Let’s start creating a unit test for testing the above queries. Before our test cases run, we have to insert some data into our database tables.

```java
public class UnrelatedEntitiesUnitTest {
    // ...

    @BeforeAll
    public static void setup() {
        // ...

        mojito = new Cocktail();
        mojito.setName("Mojito");
        mojito.setPrice(12.12);
        ginTonic = new Cocktail();
        ginTonic.setName("Gin tonic");
        ginTonic.setPrice(10.50);
        Recipe mojitoRecipe = new Recipe(); 
        mojitoRecipe.setCocktail(mojito.getName()); 
        mojitoRecipe.setInstructions("Some instructions for making a mojito cocktail!");
        entityManager.persist(mojito);
        entityManager.persist(ginTonic);
        entityManager.persist(mojitoRecipe);
      
        // ...
    }

    // ... 
}
```

In the setup method, we are saving two Cocktail entities, the mojito and the ginTonic. Then, we add a recipe for how we can make a “Mojito” cocktail.

Now, we can test the results of the queries of the previous section. We know that only the mojito cocktail has an associated Recipe entity, so we expect the various queries to return only the mojito cocktail:

```java
public class UnrelatedEntitiesUnitTest {
    // ...

    @Test
    public void givenCocktailsWithRecipe_whenQuerying_thenTheExpectedCocktailsReturned() {
        // JPA
        Cocktail cocktail = entityManager.createQuery("select c " +
          "from Cocktail c join c.recipe", Cocktail.class)
          .getSingleResult();
        verifyResult(mojito, cocktail);

        cocktail = entityManager.createQuery("select c " +
          "from Cocktail c join Recipe r " +
          "on c.name = r.cocktail", Cocktail.class).getSingleResult();
        verifyResult(mojito, cocktail);

        // QueryDSL
        cocktail = new JPAQuery<Cocktail>(entityManager).from(QCocktail.cocktail)
          .join(QCocktail.cocktail.recipe)
          .fetchOne();
        verifyResult(mojito, cocktail);

        cocktail = new JPAQuery<Cocktail>(entityManager).from(QCocktail.cocktail)
          .join(QRecipe.recipe)
          .on(QCocktail.cocktail.name.eq(QRecipe.recipe.cocktail))
          .fetchOne();
        verifyResult(mojito, cocktail);
    }

    private void verifyResult(Cocktail expectedCocktail, Cocktail queryResult) {
        assertNotNull(queryResult);
        assertEquals(expectedCocktail, queryResult);
    }

    // ...
}
```

The verifyResult method helps us to verify that the result returned from the query is equal to the expected result.

7. **One-To-Many Underlying Relationship**

Let’s change the domain of our example to show how we can join two entities with a one-to-many underlying relationship.

![JPA Query between Unrelated Entities](https://www.baeldung.com/wp-content/uploads/2020/04/one-to-many-768x234.png)

Instead of the recipes table, we have the multiple_recipes table, where we can store as many recipes as we want for the same cocktail.

```java
@Entity
@Table(name = "multiple_recipes")
public class MultipleRecipe {
    @Id
    @Column(name = "id")
    private Long id;

    @Column(name = "cocktail")
    private String cocktail;

    @Column(name = "instructions")
    private String instructions;

    // getters & setters
}
```

Now, the Cocktail entity is associated with the MultipleRecipe entity by a one-to-many underlying relationship :

```java
@Entity
@Table(name = "cocktails")
public class Cocktail {    
    // ...

    @OneToMany
    @JoinColumn(
       name = "cocktail", 
       referencedColumnName = "cocktail_name", 
       insertable = false, 
       updatable = false, 
       foreignKey = @jakarta.persistence
         .ForeignKey(value = ConstraintMode.NO_CONSTRAINT))
    private List<MultipleRecipe> recipeList;

    // getters & setters
}
```

To find and get the Cocktail entities for which we have at least one available MultipleRecipe, we can query the Cocktail entity by joining it with its associated MultipleRecipe entities.

We can do this using JPQL:

```
entityManager.createQuery("select c from Cocktail c join c.recipeList");
```

or by using the QueryDSL framework:

```
new JPAQuery(entityManager).from(QCocktail.cocktail)
  .join(QCocktail.cocktail.recipeList);
```

There is also the option to not use the recipeList field which defines the one-to-many relationship between the Cocktail and MultipleRecipe entities. Instead, we can write a join query for the two entities and determine their underlying relationship by using JPQL “on” clause:

```
entityManager.createQuery("select c "
  + "from Cocktail c join MultipleRecipe mr "
  + "on mr.cocktail = c.name");
```

Finally, we can construct the same query by using the QueryDSL framework:

```
new JPAQuery(entityManager).from(QCocktail.cocktail)
  .join(QMultipleRecipe.multipleRecipe)
  .on(QCocktail.cocktail.name.eq(QMultipleRecipe.multipleRecipe.cocktail));
```

8. **One-To-Many Join Unit Test**

Here, we’ll add a new test case for testing the previous queries. Before doing so, we have to persist some MultipleRecipe instances during our setup method:

```
public class UnrelatedEntitiesUnitTest {    
    // ...

    @BeforeAll
    public static void setup() {
        // ...
        
        MultipleRecipe firstMojitoRecipe = new MultipleRecipe();
        firstMojitoRecipe.setId(1L);
        firstMojitoRecipe.setCocktail(mojito.getName());
        firstMojitoRecipe.setInstructions("The first recipe of making a mojito!");
        entityManager.persist(firstMojitoRecipe);
        MultipleRecipe secondMojitoRecipe = new MultipleRecipe();
        secondMojitoRecipe.setId(2L);
        secondMojitoRecipe.setCocktail(mojito.getName());
        secondMojitoRecipe.setInstructions("The second recipe of making a mojito!"); 
        entityManager.persist(secondMojitoRecipe);
       
        // ...
    }

    // ... 
}
```

We can then develop a test case, where we verify that when the queries we showed in the previous section are executed, they return the Cocktail entities that are associated with at least one MultipleRecipe instance:

```java
public class UnrelatedEntitiesUnitTest {
    // ...
    
    @Test
    public void givenCocktailsWithMultipleRecipes_whenQuerying_thenTheExpectedCocktailsReturned() {
        // JPQL
        Cocktail cocktail = entityManager.createQuery("select c "
          + "from Cocktail c join c.recipeList", Cocktail.class)
          .getSingleResult();
        verifyResult(mojito, cocktail);

        cocktail = entityManager.createQuery("select c "
          + "from Cocktail c join MultipleRecipe mr "
          + "on mr.cocktail = c.name", Cocktail.class)
          .getSingleResult();
        verifyResult(mojito, cocktail);

        // QueryDSL
        cocktail = new JPAQuery<Cocktail>(entityManager).from(QCocktail.cocktail)
          .join(QCocktail.cocktail.recipeList)
          .fetchOne();
        verifyResult(mojito, cocktail);

        cocktail = new JPAQuery<Cocktail>(entityManager).from(QCocktail.cocktail)
          .join(QMultipleRecipe.multipleRecipe)
          .on(QCocktail.cocktail.name.eq(QMultipleRecipe.multipleRecipe.cocktail))
          .fetchOne();
        verifyResult(mojito, cocktail);
    }

    // ...

}
```

9. **Many-To-Many Underlying Relationship**

In this section, we choose to categorize our cocktails in our menu by their base ingredient. For example, the base ingredient of the mojito cocktail is the rum, so the rum is a cocktail category in our menu.

To depict the above in our domain, we add the category field into the Cocktail entity:

```java
@Entity
@Table(name = "menu")
public class Cocktail {
    // ...

    @Column(name = "category")
    private String category;
    
     // ...
}
```

Also, we can add the base_ingredient column to the multiple_recipes table to be able to search for recipes based on a specific drink.

```java
@Entity
@Table(name = "multiple_recipes")
public class MultipleRecipe {
    // ...
    
    @Column(name = "base_ingredient")
    private String baseIngredient;
    
    // ...
}
```

After the above, here’s our database schema:

![JPA Query between Unrelated Entities](https://www.baeldung.com/wp-content/uploads/2020/04/many_to_many-768x262.png)

Now, we have a many-to-many underlying relationship between Cocktail and MultipleRecipe entities. Many MultipleRecipe entities can be associated with many Cocktail entities that their category value is equal with the baseIngredient value of the MultipleRecipe entities.

To find and get the MultipleRecipe entities that their baseIngredient exists as a category in the Cocktail entities, we can join these two entities by using JPQL:

```
entityManager.createQuery("select distinct r " 
  + "from MultipleRecipe r " 
  + "join Cocktail c " 
  + "on r.baseIngredient = c.category", MultipleRecipe.class)
```

Or by using QueryDSL:

```
QCocktail cocktail = QCocktail.cocktail; 
QMultipleRecipe multipleRecipe = QMultipleRecipe.multipleRecipe; 
new JPAQuery(entityManager).from(multipleRecipe)
  .join(cocktail)
  .on(multipleRecipe.baseIngredient.eq(cocktail.category))
  .fetch();
```

10. **Many-To-Many Join Unit Test**

Before proceeding with our test case we have to set the category of our Cocktail entities and the baseIngredient of our MultipleRecipe entities:

```java
public class UnrelatedEntitiesUnitTest {
    // ...

    @BeforeAll
    public static void setup() {
        // ...

        mojito.setCategory("Rum");
        ginTonic.setCategory("Gin");
        firstMojitoRecipe.setBaseIngredient(mojito.getCategory());
        secondMojitoRecipe.setBaseIngredient(mojito.getCategory());

        // ...
    }

    // ... 
}
```

Then, we can verify that when the queries we showed previously are executed, they return the expected results:

```java
public class UnrelatedEntitiesUnitTest {
    // ...

    @Test
    public void givenMultipleRecipesWithCocktails_whenQuerying_thenTheExpectedMultipleRecipesReturned() {
        Consumer<List<MultipleRecipe>> verifyResult = recipes -> {
            assertEquals(2, recipes.size());
            recipes.forEach(r -> assertEquals(mojito.getName(), r.getCocktail()));
        };

        // JPQL
        List<MultipleRecipe> recipes = entityManager.createQuery("select distinct r "
          + "from MultipleRecipe r "
          + "join Cocktail c " 
          + "on r.baseIngredient = c.category",
          MultipleRecipe.class).getResultList();
        verifyResult.accept(recipes);

        // QueryDSL
        QCocktail cocktail = QCocktail.cocktail;
        QMultipleRecipe multipleRecipe = QMultipleRecipe.multipleRecipe;
        recipes = new JPAQuery<MultipleRecipe>(entityManager).from(multipleRecipe)
          .join(cocktail)
          .on(multipleRecipe.baseIngredient.eq(cocktail.category))
          .fetch();
        verifyResult.accept(recipes);
    }

    // ...
}
```
