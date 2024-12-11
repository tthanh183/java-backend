### In Expressions

1. **Overview**

We often come across problems where we need to query entities based on whether a single-valued attribute is a member of a given collection.

In this tutorial, we’ll learn how to solve this problem with the help of the Criteria API.

2. **Sample Entities**

Before we start, let’s take a look at the entities that we’re going to use in our write-up.

We have a DeptEmployee class that has a many-to-one relationship with a Department class:

```java
@Entity
public class DeptEmployee {
    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE)
    private long id;

    private String title;

    @ManyToOne
    private Department department;
}   
```

Also, the Department entity that maps to multiple DeptEmployees:

```java
@Entity
public class Department {
    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE)
    private long id;

    private String name;

    @OneToMany(mappedBy="department")
    private List<DeptEmployee> employees;
}
```

3. **The CriteriaBuilder.In**

First of all, let’s use the CriteriaBuilder interface. The in() method accepts an Expression and returns a new Predicate of the CriteriaBuilder.In type. It can be used to test whether the given expression is contained in the list of values:

```
CriteriaQuery<DeptEmployee> criteriaQuery = 
  criteriaBuilder.createQuery(DeptEmployee.class);
Root<DeptEmployee> root = criteriaQuery.from(DeptEmployee.class);
In<String> inClause = criteriaBuilder.in(root.get("title"));
for (String title : titles) {
    inClause.value(title);
}
criteriaQuery.select(root).where(inClause);
```

4. **The Expression In**

Alternatively, we can use a set of overloaded in() methods from the Expression interface:

```
criteriaQuery.select(root)
  .where(root.get("title")
  .in(titles));
```

In a contrast to the CriteriaBuilder.in(), the Expression.in() accepts a collection of values. As we can see it also simplifies our code a little bit.

5. **IN Expressions Using Subqueries**

So far, we have used collections with predefined values. Now, let’s take a look at an example when a collection is derived from an output of a subquery.

For instance, we can fetch all DeptEmployees who belong to a Department, with the specified keyword in their name:

```
Subquery<Department> subquery = criteriaQuery.subquery(Department.class);
Root<Department> dept = subquery.from(Department.class);
subquery.select(dept)
  .distinct(true)
  .where(criteriaBuilder.like(dept.get("name"), "%" + searchKey + "%"));

criteriaQuery.select(emp)
  .where(criteriaBuilder.in(emp.get("department")).value(subquery));
```

Here, we created a subquery that was then passed into the value() as an expression to search for the Department entity.
