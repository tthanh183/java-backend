### Spring Integration

Spring is an open-source framework for building enterprise-level Java applications. Spring has an applicationContext.xml file. We will use this file to provide all information including database configuration. Since database configuration is included in hibernate.cfg.xml in Hibernate, we will not need it anymore.

In Hibernate, we use StandardServiceRegistry, MetadataSources, SessionFactory to get Session, Transaction. Spring framework provides a HibernateTemplate class, where you just have to call save method to persist in database. HibernateTemplate class resides in org.springframework.orm.hibernate3 package.

Syntax:
```
// create a new student object
Student s1 = new Student( 111, "Karan", "Physics");

// save the student object in database using HibernateTemplate instance
hibernateTemplate.persist(s1);
```