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

Useful method in HibernateTemplate class:

| Method Name                      | Description                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| void persist(Object entity)      | Saves the object to the mapped table in database.                                                         |
| Serializable save(Object entity) | Saves the object to the mapped table record in database and returns id.                                   |
| void saveOrUpdate(Object entity) | Saves or Updates the table mapped to the object. If id is entered, it updates the record otherwise saves. |
| void update(Object entity)       | Updates the table mapped to the object.                                                                   |
| void delete(Object entity)       | Deletes the given object mapped to the table in database on the basis of id.                              |


### Spring Integration Example

1. **Create Mapping Class**:

```java
package com.tutorialspoint;

public class Student {

   private long id;
   private String name;
   private String dept;
   public long getId() {
      return id;
   }
   public void setId(long id) {
      this.id = id;
   }
   public String getName() {
      return name;
   }
   public void setName(String name) {
      this.name = name;
   }
   public String getDept() {
      return dept;
   }
   public void setDept(String dept) {
      this.dept = dept;
   }
}
```

2. **Create DAO Class**:

Create the DAO class which will be using HibernateTemplate class to create, update and delete student object.

```java
package com.tutorialspoint;

import org.springframework.orm.hibernate3.HibernateTemplate;

public class StudentDAO{

   HibernateTemplate template;

   public void setTemplate(HibernateTemplate template) {
      this.template = template;
   }

   public void saveStudent(Student s){
      template.save(s);
   }

   public void updateStudent(Student s){
      template.update(s);
   }

   public void deleteStudent(Student s){
      template.delete(s);
   }
}
```

3. **Create XML**:

Create Mapping XML

```
<?xml version='1.0' encoding='UTF-8'?>
   <!DOCTYPE hibernate-mapping PUBLIC
   "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
   "http://hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping>
   <class name="Student" table="student_hib">
      <id name="id">
         <generator class="assigned"></generator>
      </id>          
      <property name="name"></property>
      <property name="dept"></property>
   </class>          
</hibernate-mapping>
```

Create Application Context XML

```
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
   xmlns:p="http://www.springframework.org/schema/p"  
   xsi:schemaLocation="http://www.springframework.org/schema/beans          
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">  
   <bean id="dataSource" class=" com.mysql.cj.jdbc.MysqlDataSource">  
      <property name="driverClassName"  value=" com.mysql.cj.jdbc.Driver"></property>  
      <property name="url" value=" jdbc:mysql://localhost/TUTORIALSPOINT"></property>  
      <property name="username" value="root"></property>  
      <property name="password" value="guest123"></property>  
   </bean>  
   <bean id="localSessionFactory"  class="org.springframework.orm.hibernate3.LocalSessionFactoryBean">  
      <property name="dataSource" ref="dataSource"></property>  
      <property name="mappingResources">  
         <list>  
            <value>student.hbm.xml</value>  
         </list>  
      </property>  
      <props>  
         <prop key="hibernate.dialect"> org.hibernate.dialect.MySQL8Dialect</prop>  
         <prop key="hibernate.hbm2ddl.auto">update</prop>  
      </props>  
   </bean>  
   <bean id="hibernateTemplate" class="org.springframework.orm.hibernate3.HibernateTemplate">  
      <property name="sessionFactory" ref="localSessionFactory"></property>  
   </bean>  
   <bean id="st" class="StudentDAO">  
      <property name="template" ref="hibernateTemplate"></property>  
   </bean>  
</beans> 
```

4. **Create Application Class**:

```java
package com.tutorialspoint;

import org.springframework.beans.factory.BeanFactory;
import org.springframework.beans.factory.xml.XmlBeanFactory;
import org.springframework.core.io.ClassPathResource;
import org.springframework.core.io.Resource;

public class PersistStudent {
   public static void main(String[] args) {

      Resource r=new ClassPathResource("applicationContext.xml");
      BeanFactory factory=new XmlBeanFactory(r);

      StudentDAO dao=(StudentDAO)factory.getBean("st");

      Student s=new Student();
      s.setId(190);
      s.setName("Danny Jing");
      s.setDept("Physics");
      dao.persistStudent(s);
      System.out.println(" Successfully persisted the student. Please check your database for results."); 
	  
   }
}
```