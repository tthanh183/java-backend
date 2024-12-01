### Sessions

A Session is used to get a physical connection with a database. The Session object is lightweight and designed to be instantiated each time an interaction is needed with the database. Persistent objects are saved and retrieved through a Session object.

The session objects should not be kept open for a long time because they are not usually thread safe and they should be created and destroyed them as needed. The main function of the Session is to offer, create, read, and delete operations for instances of mapped entity classes.

Instances may exist in one of the following three states at a given point in time:
- Transient: An object is transient if it has just been instantiated using the new operator, and it is not associated with a Hibernate Session. It has no persistent representation in the database and no identifier value has been assigned.
- Persistent: An object is persistent if it has a representation in the database and an identifier value. It might just have been saved or loaded, however, it is associated with a Session.
- Detached: Once the session is closed, the persistent instance will become a detached instance. Any changes made to the detached instance will not be saved to the database.

Session Interface provides the following methods to perform the database operations:

| Method Name                                    | Description                                                                                                                              |
|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| Transaction beginTransaction()                 | It starts a new database transaction.                                                                                                    |
| void cancelQuery()                             | It cancels the execution of the query.                                                                                                   |
| void clear()                                   | It completely clear the session.                                                                                                         |
| void close()                                   | It closes the session.                                                                                                                   |
| Criteria createCriteria(Class persistentClass) | It creates a new Criteria instance, for the given entity class.                                                                          |
| Query createQuery(String queryString)          | It creates a new instance of Query for the given HQL query string.                                                                       |
| SQLQuery createSQLQuery(String queryString)    | It creates a new instance of SQLQuery for the given SQL query string.                                                                    |
| Session getCurrentSession()                    | It returns the current session associated with the thread.                                                                               |
| Object get(Class clazz, Serializable id)       | It returns the persistent instance of the given entity class with the given identifier, or null if there is no such persistent instance. |
| Serializable save(Object object)               | It persists the given transient instance, first assigning a generated identifier.                                                        |
| void saveOrUpdate(Object object)               | It either saves a transient instance or updates the persistent instance.                                                                 |
| void update(Object object)                     | It updates the persistent instance with the identifier of the given detached instance.                                                   |
| void delete(Object object)                     | It deletes a persistent instance.                                                                                                        |
