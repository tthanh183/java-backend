### Entity Life Cycle

An entity in Hibernate is an object that is mapped to a database table. An entity can be in one of the four states −

| State        | Description                                                                                                   |
|--------------|---------------------------------------------------------------------------------------------------------------|
| `Transient`  | Not persistent. Just instantiated, or maybe some methods have been called, but not associated with a Session. |
| `Persistent` | Associated with a Session. As soon as we call save or persist, the object is in persistent state.             |
| `Detached`   | Previously attached to a Session, but not anymore.                                                            |
| `Removed`    | An entity enters the removed state when it is marked for deletion from the database.                          |

1. **Transient State**:

Transient state means an entity is not persistent. It is just instantiated, or maybe some methods have been called, but not associated with a Session. Objects can enter persistent state by calling save, persist or saveOrUpdate() methods.

```
Employee employee = new Employee();
employee.setName("John Doe");
employee.setSalary(50000.0);
```

2. **Persistent State**:   

Persistent state refers to an association with a session. As soon as we call save or persist, the object is in persistent state. Persistent objects can become transient when the delete() method is called. Calling get() and load() on Session, returns a persistent entity.

```
Session session = sessionFactory.openSession();
session.beginTransaction();

Employee employee = new Employee();
employee.setName("John Doe");
employee.setSalary(50000.0);

session.save(employee); // The entity is now in the persistent //state

session.getTransaction().commit();
session.close();
```

3. **Detached State**:

Detached state refers to state where an entity was previously attached with a session, but not anymore. An entity enters the detached state when the Hibernate session that was managing it is closed. In this state, the entity is no longer associated with any session, and changes made to it are not automatically synchronized with the database.

```
Session session = sessionFactory.openSession();
session.beginTransaction();

Employee employee = session.get(Employee.class, 1L);
session.getTransaction().commit();
session.close(); // The entity is now in the detached state

employee.setSalary(60000.0); // Changes are not synchronized with the database
```

4. **Removed State**:

An entity enters the removed state when it is marked for deletion from the database. In this state, the entity is still associated with a session, but it will be deleted from the database when the transaction is committed.

```
Session session = sessionFactory.openSession();
session.beginTransaction();

Employee employee = session.get(Employee.class, 1L);
session.delete(employee); // The entity is now in the removed state

session.getTransaction().commit();
session.close();
```

Summary
![Entity Life Cycle](https://gpcoder.com/wp-content/uploads/2020/03/hibernate-lifecycle.png?v=1589391034)