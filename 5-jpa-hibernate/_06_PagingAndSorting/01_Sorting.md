### Sorting

1. **Overview**

This article illustrates the various way JPA can be used for sorting.

2. **Sorting With JPA / JQL API**

Using JQL to sort is done with the help of the Order By clause:

```
String jql ="Select f from Foo as f order by f.id";
Query query = entityManager.createQuery (jql);
```

Based on this query, JPA generates the following straighforward SQL statement:

```
Hibernate: select foo0_.id as id1_4_, foo0_.name as name2_4_ 
    from Foo foo0_ order by foo0_.id
```

Note that the SQL keywords in the JQL string are not case sensitive, but the names of the entities and their attributes are.

- Setting the Sorting Order

By default the sorting order is ascending, but it can be explicitly set in the JQL string. Just as in pure SQL the ordering options are asc and desc:

```java
String jql = "Select f from Foo as f order by f.id desc";
Query sortQuery = entityManager.createQuery(jql);
```

The generated SQL query will then include the order direction:

```
Hibernate: select foo0_.id as id1_4_, foo0_.name as name2_4_ 
    from Foo foo0_ order by foo0_.id desc
```

- Sorting by More Than One Attribute

To sort by multiple attributes, these are added to the order by clause of the JQL string:

```java
String jql ="Select f from Foo as f order by f.name asc, f.id desc";
Query sortQuery = entityManager.createQuery(jql);
```

Both sorting conditions will appear in the generated SQL query statement:

```
Hibernate: select foo0_.id as id1_4_, foo0_.name as name2_4_ 
    from Foo foo0_ order by foo0_.name asc, foo0_.id desc
```

- Setting Sorting Precedence of Null Values

The default precedence of nulls is database specific, but this is customizable through the NULLS FIRST or NULLS LAST clause in the HQL query string.

Here is a simple example – ordering by name of Foo in descending order and placing Nulls at the end:

```java
Query sortQuery = entityManager.createQuery
    ("Select f from Foo as f order by f.name desc NULLS LAST");
```

The SQL query that is generated includes the is null the 1 else 0 end clause (3rd line):

```
Hibernate: select foo0_.id as id1_4_, foo0_.BAR_ID as BAR_ID2_4_, 
    foo0_.bar_Id as bar_Id2_4_, foo0_.name as name3_4_,from Foo foo0_ order 
    by case when foo0_.name is null then 1 else 0 end, foo0_.name desc
```

- Sorting One To Many Relations

Moving past the basic examples, let’s now look at a use case involving sorting entities in a one to many relation – Bar containing a collection of Foo entities.

We want to sort the Bar entities and also their collection of Foo entities – JPA is especially simple for this task:

Sorting the Collection: Add an OrderBy annotation preceding the Foo collection in the Bar entity:

```java
@OrderBy("name ASC")
List <Foo> fooList;
```

Sorting the entity containing the collection:

```java
String jql = "Select b from Bar as b order by b.id";
Query barQuery = entityManager.createQuery(jql);
List<Bar> barList = barQuery.getResultList();
```

Note that the @OrderBy annotation is optional, but we are using it in this case because we want to sort the Foo collection of each Bar.

Lets take a look at the SQL query sent to the RDMS:

```
Hibernate: select bar0_.id as id1_0_, bar0_.name as name2_0_ from Bar bar0_ order by bar0_.id

Hibernate: 
select foolist0_.BAR_ID as BAR_ID2_0_0_, foolist0_.id as id1_4_0_, 
foolist0_.id as id1_4_1_, foolist0_.BAR_ID as BAR_ID2_4_1_, 
foolist0_.bar_Id as bar_Id2_4_1_, foolist0_.name as name3_4_1_ 
from Foo foolist0_ 
where foolist0_.BAR_ID=? order by foolist0_.name asc
```

The first query sorts the parent Bar entity. The second query is generated to sort the collection of child Foo entities belonging to Bar.

3. **Sorting With JPA Criteria Query Object API**

With JPA Criteria – the orderBy method is a “one stop” alternative to set all sorting parameters: both the order direction and the attributes to sort by can be set. Following is the method’s API:

- orderBy(CriteriaBuilder.asc): Sorts in ascending order.
- orderBy(CriteriaBuilder.desc): Sorts in descending order.

Each Order instance is created with the CriteriaBuilder object through its asc or desc methods.

Here is a quick example – sorting Foos by their name:

```
CriteriaQuery<Foo> criteriaQuery = criteriaBuilder.createQuery(Foo.class);
Root<Foo> from = criteriaQuery.from(Foo.class);
CriteriaQuery<Foo> select = criteriaQuery.select(from);
criteriaQuery.orderBy(criteriaBuilder.asc(from.get("name")));
```

The argument to the get method is case sensitive, since it needs to match the name of the attribute.

As opposed to simple JQL, the JPA Criteria Query Object API forces an explicit order direction in the query. Notice in the last line of this code snippet that the criteriaBuilder object specifies the sorting order to be ascending by calling its asc method.

When the above code is executed, JPA generates the SQL query shown below. JPA Criteria Object generates an SQL statement with an explicit asc clause:

```
Hibernate: select foo0_.id as id1_4_, foo0_.name as name2_4_
    from Foo foo0_ order by foo0_.name asc
```

-  Sorting by More Than One Attribute

To sort by more than one attribute simply pass an Order instance to the orderBy method for each attribute to sort by.

Here is a quick example – sorting by name and id, in asc and desc order, respectively:

```
CriteriaQuery<Foo> criteriaQuery = criteriaBuilder.createQuery(Foo.class);
Root<Foo> from = criteriaQuery.from(Foo.class); 
CriteriaQuery<Foo> select = criteriaQuery.select(from); 
criteriaQuery.orderBy(criteriaBuilder.asc(from.get("name")),
    criteriaBuilder.desc(from.get("id")));
```

The corresponding SQL query is shown below:

```
Hibernate: select foo0_.id as id1_4_, foo0_.name as name2_4_ 
    from Foo foo0_ order by foo0_.name asc, foo0_.id desc
```