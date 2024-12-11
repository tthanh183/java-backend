### Criteria Queries

1. **Overview**

In this tutorial, we’ll discuss a very useful JPA feature — Criteria Queries.

It enables us to write queries without doing raw SQL as well as gives us some object-oriented control over the queries, which is one of the main features of Hibernate. The Criteria API allows us to build up a criteria query object programmatically, where we can apply different kinds of filtration rules and logical conditions.

Since Hibernate 5.2, the Hibernate Criteria API is deprecated, and new development is focused on the JPA Criteria API. We’ll explore how to use Hibernate and JPA to build Criteria Queries.

2. **Maven Dependencies**

To illustrate the API, we’ll use the reference JPA implementation Hibernate.

To use Hibernate, we’ll make sure to add the latest version of it to our pom.xml file:

```xml
<dependency>
    <groupId>org.hibernate.orm</groupId>
    <artifactId>hibernate-core</artifactId>   
    <version>6.5.2.Final</version>
</dependency>
```

We can find the latest version of Hibernate here.

3. **Simple Example Using Criteria**

Let’s start by looking at how to retrieve data using Criteria Queries. We’ll look at how to get all the instances of a particular class from the database.

We have an Item class that represents the tuple “ITEM” in the database:

```java
public class Item implements Serializable {

    private Integer itemId;
    private String itemName;
    private String itemDescription;
    private Integer itemPrice;

   // standard setters and getters
}
```

Let’s look at a simple criteria query that will retrieve all the rows of “ITEM” from the database:

```
Session session = HibernateUtil.getHibernateSession();
CriteriaBuilder cb = session.getCriteriaBuilder();
CriteriaQuery<Item> cr = cb.createQuery(Item.class);
Root<Item> root = cr.from(Item.class);
cr.select(root);

Query<Item> query = session.createQuery(cr);
List<Item> results = query.getResultList();
```

The above query is a simple demonstration of how to get all the items. Let’s see it step by step:

    - Create an instance of Session from the SessionFactory object  
    - Create an instance of CriteriaBuilder by calling the getCriteriaBuilder() method  
    - Create an instance of CriteriaQuery by calling the CriteriaBuilder createQuery() method  
    - Create an instance of Query by calling the Session createQuery() method  
    - Call the getResultList() method of the query object, which gives us the results  

Now that we’ve covered the basics, let’s move on to some of the features of criteria query.

- Using Expressions

The CriteriaBuilder can be used to restrict query results based on specific conditions, by using CriteriaQuery where() method and providing Expressions created by CriteriaBuilder.

Let’s see some examples of commonly used Expressions.

In order to get items having a price of more than 1000:

```
cr.select(root).where(cb.gt(root.get("itemPrice"), 1000));
```

Next, getting items having itemPrice less than 1000:

```
cr.select(root).where(cb.lt(root.get("itemPrice"), 1000));
```

Items having itemName contain Chair:

```
cr.select(root).where(cb.like(root.get("itemName"), "%chair%"));
```

Records having itemPrice between 100 and 200:

```
cr.select(root).where(cb.between(root.get("itemPrice"), 100, 200));
```

Items having itemName in Skate Board, Paint and Glue:

```
cr.select(root).where(root.get("itemName").in("Skate Board", "Paint", "Glue"));
```

To check if the given property is null:

```
cr.select(root).where(cb.isNull(root.get("itemDescription")));
```

To check if the given property is not null:

```
cr.select(root).where(cb.isNotNull(root.get("itemDescription")));
```

We can also use the methods isEmpty() and isNotEmpty() to test if a List within a class is empty or not.

Additionally, we can combine two or more of the above comparisons. The Criteria API allows us to easily chain expressions:

```
Predicate[] predicates = new Predicate[2];
predicates[0] = cb.isNull(root.get("itemDescription"));
predicates[1] = cb.like(root.get("itemName"), "chair%");
cr.select(root).where(predicates);
```

To add two expressions with logical operations:

```
Predicate greaterThanPrice = cb.gt(root.get("itemPrice"), 1000);
Predicate chairItems = cb.like(root.get("itemName"), "Chair%");
```

Items with the above-defined conditions joined with Logical OR:

```
cr.select(root).where(cb.or(greaterThanPrice, chairItems));
```

To get items matching with the above-defined conditions joined with Logical AND:

```
cr.select(root).where(cb.and(greaterThanPrice, chairItems));
```

- Sorting

Now that we know the basic usage of Criteria, let’s look at the sorting functionalities of Criteria.

In the following example, we order the list in ascending order of the name and then in descending order of the price:

```
cr.orderBy(
  cb.asc(root.get("itemName")), 
  cb.desc(root.get("itemPrice")));
```

In the next section, we will have a look at how to do aggregate functions.

- Projections, Aggregates and Grouping Functions

Now let’s see the different aggregate functions.

Get row count:

```
CriteriaQuery<Long> cr = cb.createQuery(Long.class);
Root<Item> root = cr.from(Item.class);
cr.select(cb.count(root));
Query<Long> query = session.createQuery(cr);
List<Long> itemProjected = query.getResultList();
```

The following is an example of aggregate functions — Aggregate function for Average:

```
CriteriaQuery<Double> cr = cb.createQuery(Double.class);
Root<Item> root = cr.from(Item.class);
cr.select(cb.avg(root.get("itemPrice")));
Query<Double> query = session.createQuery(cr);
List avgItemPriceList = query.getResultList();
```

Other useful aggregate methods are sum(), max(), min(), count(), etc.

- CriteriaUpdate

Starting from JPA 2.1, there’s support for performing database updates using the Criteria API.

CriteriaUpdate has a set() method that can be used to provide new values for database records:

```
CriteriaUpdate<Item> criteriaUpdate = cb.createCriteriaUpdate(Item.class);
Root<Item> root = criteriaUpdate.from(Item.class);
criteriaUpdate.set("itemPrice", newPrice);
criteriaUpdate.where(cb.equal(root.get("itemPrice"), oldPrice));

Transaction transaction = session.beginTransaction();
session.createQuery(criteriaUpdate).executeUpdate();
transaction.commit();
```

In the above snippet, we create an instance of CriteriaUpdate<Item> from the CriteriaBuilder and use its set() method to provide new values for the itemPrice. In order to update multiple properties, we just need to call the set() method multiple times.

- CriteriaDelete

CriteriaDelete enables a delete operation using the Criteria API.

We just need to create an instance of CriteriaDelete and use the where() method to apply restrictions:

```
CriteriaDelete<Item> criteriaDelete = cb.createCriteriaDelete(Item.class);
Root<Item> root = criteriaDelete.from(Item.class);
criteriaDelete.where(cb.greaterThan(root.get("itemPrice"), targetPrice));

Transaction transaction = session.beginTransaction();
session.createQuery(criteriaDelete).executeUpdate();
transaction.commit();
```

- Using the CriteriaDefinition Utility

Hibernate 6.3.0 provides a CriteriaDefinition class to simplify building criteria queries. It allows defining an initializer block of an anonymous subclass where all operations of CriteriaBuilder and CriteriaQuery can be invoked without specifying the target object.

Here’s a criteria query that retrieves all rows of “ITEM” using CriteriaDefinition:

```
final Session session = HibernateUtil.getHibernateSession();
final SessionFactory sessionFactory = HibernateUtil.getHibernateSessionFactory();
CriteriaDefinition<Item> query = new CriteriaDefinition<>(sessionFactory, Item.class) {
    {
        JpaRoot<Item> item = from(Item.class);
    }
};
List<Item> items = session.createSelectionQuery(query).list();
```


In the code above, we create the CriterianDefinition object to build a query, passing SessionFactory and Item class as parameters.

Also, we define an initializer block where we construct the query.

Finally, we store the queried data in a List by invoking the createSelectionQuery() method on a Session object.

Next, let’s see some examples of common criteria queries using CriteriaDefinition. First, let’s get Items having a price greater than 1000:

```
CriteriaDefinition<Item> query = new CriteriaDefinition<>(sessionFactory, Item.class) {
    {
        JpaRoot<Item> item = from(Item.class);
        where(gt(item.get("itemPrice"), 1000));
    }
};
List<Item> items = session.createSelectionQuery(query).list();
```

First, we select the Item. Next, we invoke methods named where() and gt() to further specify that the itemPrice must be greater than 1000.

Next, let’s get Items with price between 100 and 200:

```
where(between(item.get("itemPrice"), 100, 200));
```

In the code above, we use the between() method to put the retrieved data within a range.

We can easily construct criteria queries by building them in a CriteriaDefinition initializer block.

4. **Fetch Join Using JPA Criteria API**

The fetch join operation retrieves data from related entities and also initializes these related entities in a single database round trip. Unlike the Join keyword which may result in additional database queries, fetch join performs a single query that includes both the primary entity and its associated entities. This prevents the N+1 problem and improves performance.

- Entities

To demonstrate how to perform fetch join, let’s define a Player entity:

```java
@Entity
class Player {
    @Id
    private int id;
    private String playerName;
    private String teamName;
    private int age;

    @ManyToOne
    @JoinColumn(name = "league_id")
    private League league;

    // constructor, getters, setters, toString  
}
```

The Player entity has a @ManyToOne relationship with the League entity.

Here’s the League entity:

```java
@Entity
class League {
    @Id
    private int id;
    private String name;

    @OneToMany(mappedBy = "league")
    private List<Player> players = new ArrayList<>();
    // constructor, getters, setters, toString  
}
```

We mapped the League entity to the Player entity using the league_id as a foreign key.

- Perform Fetch Join Operation

Let’s fetch players based on the league they belong to:


```
CriteriaQuery<Player> query = cb.createQuery(Player.class);
Root<Player> root = query.from(Player.class);
root.fetch("league", JoinType.LEFT);
query.where(cb.equal(root.get("league").get("name"), "Premier League"));
List<Player> playerList = session.createQuery(query).getResultList();
```

In the code above, we create a Root object, which selects the Player entity. Next, we invoke the fetch() method on root and specify the league field which defines the relationship between Player and League. We also specify JoinType.LEFT to perform a left join.

Finally, we perform a query to select players who are in the Premier League. This single query fetches the Player entity along with its associated League entity.

5. **Advantages Over HQL**

In the previous sections, we covered how to use Criteria Queries.

Clearly, the main and most hard-hitting advantage of Criteria Queries over HQL is the nice, clean, object-oriented API.

We can simply write more flexible, dynamic queries compared to plain HQL. The logic can be refactored with the IDE and has all the type-safety benefits of the Java language itself.

Of course, there are some disadvantages as well, especially around more complex joins.

So, we generally have to use the best tool for the job — that can be the Criteria API in most cases, but there are definitely cases where we’ll have to go lower level.

6. **Calling a User-Defined Function**

In JPA’s Criteria API, we can call user-defined functions directly in our queries using the CriteriaBuilder‘s function method.

Let’s assume we have a user-defined function in our database called calculate_discount, which takes an itemPrice as input and returns the discounted price:

```
CREATE ALIAS calculate_discount AS $$
    double calculateDiscount(double itemPrice) {
        double discountRate = 0.1;
        return itemPrice - (itemPrice * discountRate);
    }
$$;
```

Here’s how we can call this function within a CriteriaQuery to fetch items with a discount price below a certain threshold:

```
CriteriaBuilder cb = session.getCriteriaBuilder();
CriteriaQuery<Item> query = cb.createQuery(Item.class);
Root<Item> root = query.from(Item.class);
Expression<Double> discountPrice = cb.function("calculate_discount", Double.class, root.get("itemPrice"));
query.select(root).where(cb.lt(discountPrice, 500.0));

List<Item> items = session.createQuery(query).getResultList();

assertNotNull(items, "Discounted items should not be null");
assertFalse(items.isEmpty(), "There should be discounted items returned");
assertEquals(3, items.size());
```

First, we create a CriteriaQuery for the Item entity. Then, we use the CriteriaBuilder‘s function() method to call our custom function calculate_discount. This method allows us to specify the name of the function, the expected return type, and the arguments to pass to the function.

In our example, we pass the itemPrice field as an argument to the user-defined calculate_discount function. Finally, we use the select() method to specify that we want to retrieve the result of this function call as part of our query. After building the query, we execute it using the getResultList() method, which returns a list of discounted prices.

In practice, calling user-defined functions in CriteriaQuery can be particularly useful for applying complex business logic or calculations that are more efficiently handled by the database itself.
