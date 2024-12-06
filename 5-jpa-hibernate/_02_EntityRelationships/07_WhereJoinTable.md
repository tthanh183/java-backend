### @WhereJoinTable

1. **Overview**:

Using an Object Relational Mapping tool, like Hibernate, makes it easy to read our data into objects, but can make forming our queries difficult with complex data models.

The many-to-many relationship is always challenging, but it can be more challenging when we wish to acquire related entities based on some property of the relation itself.

In this tutorial, we are going to look at how to solve this problem using Hibernate’s @WhereJoinTable annotation.

2. **Basic @ManyToMany Relation**:

Let’s start with a simple @ManyToMany relationship. We’ll need domain model entities, a relation entity, and some sample test data.

- Domain Model:

Let’s imagine we have two simple entities, User and Group, which are associated as @ManyToMany:

```java
@Entity(name = "users")
public class User {

    @Id
    @GeneratedValue
    private Long id;
    private String name;

    @ManyToMany
    private List<Group> groups = new ArrayList<>();

    // standard getters and setters

}
```

```java
@Entity
public class Group {

    @Id
    @GeneratedValue
    private Long id;
    private String name;

    @ManyToMany(mappedBy = "groups")
    private List<User> users = new ArrayList<>();

    // standard getters and setters

}
```

As we can see, our User entity can be a member of more than one Group entity. Similarly, a Group entity can contain more than one User entity.

- Relation Entity:

For @ManyToMany associations, we need a separate database table called a relation table. The relation table needs to contain at least two columns: The primary keys of the related User and Group entities.

With only the two primary key columns, our Hibernate mapping can represent this relation table.

However, if we need to put additional data in the relation table, we should also define a relation entity for the many-to-many relationship itself.

Let’s create UserGroupRelation class to do this:

```java
@Entity(name = "r_user_group")
public class UserGroupRelation implements Serializable {

    @Id
    @Column(name = "user_id", insertable = false, updatable = false)
    private Long userId;

    @Id
    @Column(name = "group_id", insertable = false, updatable = false)
    private Long groupId;

}
```

Here we’ve named the entity r_user_group so we can reference it later.

For our extra data, let’s say we want to store every User‘s role for each Group. So, we’ll create UserGroupRole enumeration:

```java
public enum UserGroupRole {
    MEMBER, MODERATOR
}
```

Next, we’ll add a role property to UserGroupRelation:

```java
@Enumerated(EnumType.STRING)
private UserGroupRole role;
```

Finally, to configure it properly, we need to add the @JoinTable annotation on User‘s groups collection. Here we’ll specify the join table name using r_user_group, the entity name of UserGroupRelation:

```java
@ManyToMany
@JoinTable(
    name = "r_user_group",
    joinColumns = @JoinColumn(name = "user_id"),
    inverseJoinColumns = @JoinColumn(name = "group_id")
)
private List<Group> groups = new ArrayList<>();
```

- Sample Data:

For our integration tests, let’s define some sample data:

```java
public void setUp() {
    session = sessionFactory.openSession();
    session.beginTransaction();
    
    user1 = new User("user1");
    user2 = new User("user2");
    user3 = new User("user3");

    group1 = new Group("group1");
    group2 = new Group("group2");

    session.save(group1);
    session.save(group2);

    session.save(user1);
    session.save(user2);
    session.save(user3);

    saveRelation(user1, group1, UserGroupRole.MODERATOR);
    saveRelation(user2, group1, UserGroupRole.MODERATOR);
    saveRelation(user3, group1, UserGroupRole.MEMBER);

    saveRelation(user1, group2, UserGroupRole.MEMBER);
    saveRelation(user2, group2, UserGroupRole.MODERATOR);
}

private void saveRelation(User user, Group group, UserGroupRole role) {

    UserGroupRelation relation = new UserGroupRelation(user.getId(), group.getId(), role);
    
    session.save(relation);
    session.flush();
    session.refresh(user);
    session.refresh(group);
}

```

As we can see, user1 and user2 are in two groups. Also, we should notice that while user1 is MODERATOR on group1, at the same time it has a MEMBER role on group2.

3. **Fetching @ManyToMany Relations**:

Now we’ve properly configured our entities, let’s fetch the groups of the User entity.

- Simple Fetch:

In order to fetch groups, we can simply invoke the getGroups() method of User inside an active Hibernate session:

```java
List<Group> groups = user1.getGroups();
```

Our output for groups will be:

```
[Group [name=group1], Group [name=group2]]    
```

- Custom Filter on a Relation Entity:

We can use the @WhereJoinTable annotation to directly acquire only filtered groups.

Let’s define a new property as moderatorGroups and put the @WhereJoinTable annotation on it. When we access the related entities via this property, it will only contain groups of which our user is MODERATOR.

We’ll need to add a SQL where clause to filter the groups by the MODERATOR role:

```java
@WhereJoinTable(clause = "role='MODERATOR'")
@ManyToMany
@JoinTable(
    name = "r_user_group",
    joinColumns = @JoinColumn(name = "user_id"),
    inverseJoinColumns = @JoinColumn(name = "group_id")
)
private List<Group> moderatorGroups = new ArrayList<>();
```

Thus, we can easily get the groups with the specified SQL where clause applied:

```java
List<Group> groups = user1.getModeratorGroups();
```

Our output will be the groups on which the user has only the role of MODERATOR:

```
[Group [name=group1]]
```
