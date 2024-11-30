### Architecture

Hibernate has a layered architecture which helps the user to operate without having to know the underlying APIs. Hibernate makes use of the database and configuration data to provide persistence services (and persistent objects) to the application.

![Hibernate Architecture](https://www.tutorialspoint.com/hibernate/images/hibernate_architecture.jpg)

Hibernate uses various existing Java APIs, like JDBC, Java Transaction API(JTA), and Java Naming and Directory Interface (JNDI). JDBC provides a rudimentary level of abstraction of functionality common to relational databases, allowing almost any database with a JDBC driver to be supported by Hibernate. JNDI and JTA allow Hibernate to be integrated with J2EE application servers.

Following section gives brief description of each of the class objects involved in Hibernate Application Architecture.

- Configuration Object: The Configuration object is the first Hibernate object you create in any Hibernate application. It is usually created only once during application initialization. It represents a configuration or properties file required by the Hibernate.
    - Database Connection: This is handled through one or more configuration files supported by Hibernate. These files are hibernate.properties and hibernate.cfg.xml.
    - Class Mapping setup: This component creates the connection between the Java classes and database tables.
- SessionFactory Object: The SessionFactory is a factory for Session objects. The SessionFactory is thread-safe and is used by all the threads of an application. It is used to get a Session object, which will be used to save the model objects. It holds the data of hibernate.cfg.xml file.
- Session Object: A Session is used to get a physical connection with a database. The Session object is lightweight and designed to be instantiated each time an interaction is needed with the database. Persistent objects are saved and retrieved through a Session object.
- Transaction Object: The transaction object is used to begin and end the transaction. The transaction object is an optional parameter for a session. Transactions are required to ensure data integrity and consistency.
- Query Object: The Query object is used to retrieve data from the database. It is an optional object and may be used by the client applications.
- Criteria Object: The Criteria object is used to create and execute object-oriented criteria queries to retrieve objects.