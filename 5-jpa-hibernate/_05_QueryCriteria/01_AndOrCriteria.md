### And/Or Criteria Predicate

1. **Overview**

The JPA Criteria API can easily be used to add multiple AND/OR conditions when querying records in a database. In this tutorial, we’ll explore a quick example of JPA criteria queries that combine multiple AND/OR predicates.

If you’re not already familiar with predicates, we suggest reading about the basic JPA criteria queries first.

2. **Sample Application**

For our examples, we’ll consider an inventory of Item entities, each having an id, name, color, and grade:

```java
@Entity
public class Item {

    @Id
    private Long id;
    private String color;
    private String grade;
    private String name;
    
    // standard getters and setters
}
```

3. **Combining Two OR Predicates Using an AND Predicate**

Let’s consider a scenario where we need to find items having both:

- A color of “Red” or “Blue” and a grade of “A” or “B”

We can easily do this using JPA Criteria API’s and() and or() compound predicates.

To begin, we’ll set up our query:

```java
CriteriaBuilder criteriaBuilder = entityManager.getCriteriaBuilder();
CriteriaQuery<Item> criteriaQuery = criteriaBuilder.createQuery(Item.class);
Root<Item> itemRoot = criteriaQuery.from(Item.class);
```

Now we’ll need to build a Predicate to find items having a blue or red color:

```java
Predicate predicateForBlueColor
  = criteriaBuilder.equal(itemRoot.get("color"), "blue");
Predicate predicateForRedColor
  = criteriaBuilder.equal(itemRoot.get("color"), "red");
Predicate predicateForColor
  = criteriaBuilder.or(predicateForBlueColor, predicateForRedColor);
```

Next, we’ll build a Predicate to find items of grade A or B:

```java
Predicate predicateForGradeA
  = criteriaBuilder.equal(itemRoot.get("grade"), "A");
Predicate predicateForGradeB
  = criteriaBuilder.equal(itemRoot.get("grade"), "B");
Predicate predicateForGrade
  = criteriaBuilder.or(predicateForGradeA, predicateForGradeB);
```

Finally, we’ll define the AND Predicate of these two, apply the where() method, and execute our query:

```java
Predicate finalPredicate
  = criteriaBuilder.and(predicateForColor, predicateForGrade);
criteriaQuery.where(finalPredicate);
List<Item> items = entityManager.createQuery(criteriaQuery).getResultList();
```

4. **Combining Two AND Predicates Using an OR Predicate**

Conversely, let’s consider a scenario where we need to find items having either:

- A color of “Red” and a grade of "D" or a color of “Blue” and a grade of "B"

The logic is quite similar, but here we create two AND Predicates first, and then combine them using an OR Predicate:

```java
CriteriaBuilder criteriaBuilder = entityManager.getCriteriaBuilder();
CriteriaQuery<Item> criteriaQuery = criteriaBuilder.createQuery(Item.class);
Root<Item> itemRoot = criteriaQuery.from(Item.class);

Predicate predicateForBlueColor
  = criteriaBuilder.equal(itemRoot.get("color"), "red");
Predicate predicateForGradeA
  = criteriaBuilder.equal(itemRoot.get("grade"), "D");
Predicate predicateForBlueColorAndGradeA
  = criteriaBuilder.and(predicateForBlueColor, predicateForGradeA);

Predicate predicateForRedColor
  = criteriaBuilder.equal(itemRoot.get("color"), "blue");
Predicate predicateForGradeB
  = criteriaBuilder.equal(itemRoot.get("grade"), "B");
Predicate predicateForRedColorAndGradeB
  = criteriaBuilder.and(predicateForRedColor, predicateForGradeB);

Predicate finalPredicate
  = criteriaBuilder
  .or(predicateForBlueColorAndGradeA, predicateForRedColorAndGradeB);
criteriaQuery.where(finalPredicate);
List<Item> items = entityManager.createQuery(criteriaQuery).getResultList();
```
