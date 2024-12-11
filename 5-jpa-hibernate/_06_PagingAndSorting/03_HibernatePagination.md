### Hibernate Pagination

1. **Overview**

This article is a quick introduction to Pagination in Hibernate. We will look at the standard HQL as well as the ScrollableResults API, and finally, at pagination with Hibernate Criteria.

2. **Pagination With HQL and setFirstResult, setMaxResults API**

The simplest and most common way to do pagination in Hibernate is using HQL:

```
Session session = sessionFactory.openSession();
Query<Foo> query = session.createQuery("From Foo", Foo.class);
query.setFirstResult(0);
query.setMaxResults(10);
List<Foo> fooList = fooList = query.list();
```

This example is using a basic Foo entity and is very similar to the JPA with JQL implementation – the only difference being the query language.

If we turn on logging for Hibernate, we’ll see the following SQL being run:

```
Hibernate: 
    select
        foo0_.id as id1_1_,
        foo0_.name as name2_1_ 
    from
        Foo foo0_ limit ?
```

- The Total Count and the Last Page 

A pagination solution is not complete without knowing the total number of entities:

```
String countQ = "Select count (f.id) from Foo f";
Query<Long> countQuery = session.createQuery(countQ, Long.class);
Long countResults = countQuery.uniqueResult();
```

And lastly, from the total number and a given page size, we can calculate the last page:

```
int pageSize = 10;
int lastPageNumber = (int) (Math.ceil(countResults / pageSize));
```

At this point we can look at a complete example for pagination, where we are calculating the last page and then retrieving it:

```java
@Test
public void givenEntitiesExist_whenRetrievingLastPage_thenCorrectSize() {
    int pageSize = 10;
    String countQ = "Select count (f.id) from Foo f";
    Query<Long> countQuery = session.createQuery(countQ, Long.class);
    Long countResults = (Long) countQuery.uniqueResult();
    int lastPageNumber = (int) (Math.ceil(countResults / pageSize));

    Query<Foo> selectQuery = session.createQuery("From Foo", Foo.class);
    selectQuery.setFirstResult((lastPageNumber - 1) * pageSize);
    selectQuery.setMaxResults(pageSize);
    List<Foo> lastPage = selectQuery.list();

    assertThat(lastPage, hasSize(lessThan(pageSize + 1)));
}
```

3. **Pagination With Hibernate Using HQL and the ScrollableResults API**

Using ScrollableResults to implement pagination has the potential to reduce database calls. This approach streams the result set as the program scrolls through it, therefore eliminating the need to repeat the query to fill each page:

```
String hql = "FROM Foo f order by f.name";
Query query = session.createQuery(hql);
int pageSize = 10;

ScrollableResults resultScroll = query.scroll(ScrollMode.FORWARD_ONLY);
resultScroll.first();
resultScroll.scroll(0);
List<Foo> fooPage = Lists.newArrayList();
int i = 0;
while (pageSize > i++) {
    fooPage.add((Foo) resultScroll.get(0));
    if (!resultScroll.next())
        break;
}
```

This method is not only time-efficient (only one database call), but it allows the user to get access to the total count of the result set without an additional query:

```
resultScroll.last();
int totalResults = resultScroll.getRowNumber() + 1;
```

On the other hand, keep in mind that, although scrolling is quite efficient, a large window may take up a decent amount of memory.

4. **Pagination With Hibernate Using the Criteria API**

Finally, let’s look at a more flexible solution – using criteria:

```
CriteriaQuery<Foo> selectQuery = session.getCriteriaBuilder().createQuery(Foo.class);
selectQuery.from(Foo.class);
SelectionQuery<Foo> query = session.createQuery(selectQuery);
query.setFirstResult(0);
query.setMaxResults(pageSize);
List<Foo> firstPage = query.list();
```

Hibernate Criteria query API makes it very simple to also get the total count :

```
HibernateCriteriaBuilder qb = session.getCriteriaBuilder();
CriteriaQuery<Long> cq = qb.createQuery(Long.class);
cq.select(qb.count(cq.from(Foo.class)));
final Long count = session.createQuery(cq).getSingleResult();
```

As you can see, using this API will result in minimally more verbose code than plain HQL, but the API is fully type safe and a lot more flexible.
