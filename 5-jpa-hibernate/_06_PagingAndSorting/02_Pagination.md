### Pagination

1. **Overview**

This article illustrates how to implement pagination in the Java Persistence API.

It explains how to do paging with basic JQL and with the more type-safe Criteria-based API’s, discussing the advantages and known issues of each implementation.

2. **Pagination With JQL and the setFirstResult(), setMaxResults() API**

The simplest way to implement pagination is to use the Java Query Language – create a query and configure it via setMaxResults and setFirstResult:

```
Query query = entityManager.createQuery("From Foo");
int pageNumber = 1;
int pageSize = 10;
query.setFirstResult((pageNumber-1) * pageSize); 
query.setMaxResults(pageSize);
List <Foo> fooList = query.getResultList();
```

The API is simple:  
    - setFirstResult(int): Sets the offset position in the result set to start pagination  
    - setMaxResults(int): Sets the maximum number of entities that should be included in the page  

- The Total Count and the Last Page 

For a more complete pagination solution, we’ll also need to get the total result count:

```
Query queryTotal = entityManager.createQuery
    ("Select count(f.id) from Foo f");
long countResult = (long)queryTotal.getSingleResult();
```

Calculating the last page is also very useful:

```java
int pageSize = 10;
int pageNumber = (int) ((countResult / pageSize) + 1);
```

Notice that this approach to getting the total count of the result set does require an additional query (for the count).

3. **Pagination With JQL Using the Id’s of Entities**

A simple alternative pagination strategy is to first retrieve the full ids and then – based on these – retrieve the full entities. This allows for better control of entity fetching – but it also means that it needs to load the entire table to retrieve the ids:

```
Query queryForIds = entityManager.createQuery(
  "Select f.id from Foo f order by f.lastName");
List<Integer> fooIds = queryForIds.getResultList();
Query query = entityManager.createQuery(
  "Select f from Foo e where f.id in :ids");
query.setParameter("ids", fooIds.subList(0,10));
List<Foo> fooList = query.getResultList();
```

Finally, also note that it requires 2 distinct queries to retrieve the full results.

4. **Pagination With JPA Using Criteria API**

Next, let’s look at how we can leverage the JPA Criteria API to implement pagination:

```
int pageSize = 10;
CriteriaBuilder criteriaBuilder = entityManager
  .getCriteriaBuilder();
CriteriaQuery<Foo> criteriaQuery = criteriaBuilder
  .createQuery(Foo.class);
Root<Foo> from = criteriaQuery.from(Foo.class);
CriteriaQuery<Foo> select = criteriaQuery.select(from);
TypedQuery<Foo> typedQuery = entityManager.createQuery(select);
typedQuery.setFirstResult(0);
typedQuery.setMaxResults(pageSize);
List<Foo> fooList = typedQuery.getResultList();
```

This is useful when the aim is to create dynamic, failure-safe queries. In contrast to “hard-coded”, “string-based” JQL or HQL queries, JPA Criteria reduces run-time failures because the compiler dynamically checks for query errors.

With JPA Criteria getting the total number of entities in simple enough:

```
CriteriaQuery<Long> countQuery = criteriaBuilder
  .createQuery(Long.class);
countQuery.select(criteriaBuilder.count(
  countQuery.from(Foo.class)));
Long count = entityManager.createQuery(countQuery)
  .getSingleResult();
```

The end result is a full pagination solution, using the JPA Criteria API:

```
int pageNumber = 1;
int pageSize = 10;
CriteriaBuilder criteriaBuilder = entityManager.getCriteriaBuilder();

CriteriaQuery<Long> countQuery = criteriaBuilder
  .createQuery(Long.class);
countQuery.select(criteriaBuilder
  .count(countQuery.from(Foo.class)));
Long count = entityManager.createQuery(countQuery)
  .getSingleResult();

CriteriaQuery<Foo> criteriaQuery = criteriaBuilder
  .createQuery(Foo.class);
Root<Foo> from = criteriaQuery.from(Foo.class);
CriteriaQuery<Foo> select = criteriaQuery.select(from);

TypedQuery<Foo> typedQuery = entityManager.createQuery(select);
while (pageNumber < count.intValue()) {
    typedQuery.setFirstResult(pageNumber - 1);
    typedQuery.setMaxResults(pageSize);
    System.out.println("Current page: " + typedQuery.getResultList());
    pageNumber += pageSize;
}
```
