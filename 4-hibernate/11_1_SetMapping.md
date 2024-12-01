### Set Mapping

A Set is a java collection that does not contain any duplicate element. More formally, sets contain no pair of elements e1 and e2 such that e1.equals(e2), and at most, one null element. So, objects to be added to a set must implement both the equals() and hashCode() methods so that Java can determine whether any two elements/objects are identical.

A Set is mapped with a <set> element in the mapping table and initialized with java.util.HashSet. You can use Set collection in your class when there is no duplicate element required in the collection.

1. **Define RDBMS Table**

```sql
create table EMPLOYEE (
   id INT NOT NULL auto_increment,
   first_name VARCHAR(20) default NULL,
   last_name  VARCHAR(20) default NULL,
   salary     INT  default NULL,
   PRIMARY KEY (id)
);
```

Further, assume each employee can have one or more certificate associated with him/her. So, we will store certificate related information in a separate table having the following structure −

```sql
create table CERTIFICATE (
   id INT NOT NULL auto_increment,
   certificate_name VARCHAR(30) default NULL,
   employee_id INT default NULL,
   PRIMARY KEY (id)
);
```

There will be one-to-many relationship between EMPLOYEE and CERTIFICATE objects −

2. **Define POJO Class**

```java
import java.util.Set;

public class Employee {
   private int id;
   private String firstName; 
   private String lastName;   
   private int salary;
   private Set certificates;

   public Employee() {}
   
   public Employee(String fname, String lname, int salary) {
      this.firstName = fname;
      this.lastName = lname;
      this.salary = salary;
   }
   
   public int getId() {
      return id;
   }
   
   public void setId( int id ) {
      this.id = id;
   }
   
   public String getFirstName() {
      return firstName;
   }
   
   public void setFirstName( String first_name ) {
      this.firstName = first_name;
   }
   
   public String getLastName() {
      return lastName;
   }
   
   public void setLastName( String last_name ) {
      this.lastName = last_name;
   }
   
   public int getSalary() {
      return salary;
   }
   
   public void setSalary( int salary ) {
      this.salary = salary;
   }

   public Set getCertificates() {
      return certificates;
   }
   
   public void setCertificates( Set certificates ) {
      this.certificates = certificates;
   }
}
```

Now let us define another POJO class corresponding to CERTIFICATE table so that certificate objects can be stored and retrieved into the CERTIFICATE table. This class should also implement both the equals() and hashCode() methods so that Java can determine whether any two elements/objects are identical.

```java
public class Certificate {
   private int id;
   private String name; 

   public Certificate() {}
   
   public Certificate(String name) {
      this.name = name;
   }
   
   public int getId() {
      return id;
   }
   
   public void setId( int id ) {
      this.id = id;
   }
   
   public String getName() {
      return name;
   }
   
   public void setName( String name ) {
      this.name = name;
   }
   
   public boolean equals(Object obj) {
      if (obj == null) return false;
      if (!this.getClass().equals(obj.getClass())) return false;

      Certificate obj2 = (Certificate)obj;
      if((this.id == obj2.getId()) && (this.name.equals(obj2.getName()))) {
         return true;
      }
      return false;
   }
   
   public int hashCode() {
      int tmp = 0;
      tmp = ( id + name ).hashCode();
      return tmp;
   }
}
```

3. **Define Mapping File**

```
<?xml version = "1.0" encoding = "utf-8"?>
<!DOCTYPE hibernate-mapping PUBLIC 
"-//Hibernate/Hibernate Mapping DTD//EN"
"http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd"> 

<hibernate-mapping>
   <class name = "Employee" table = "EMPLOYEE">
      
      <meta attribute = "class-description">
         This class contains the employee detail. 
      </meta>
      
      <id name = "id" type = "int" column = "id">
         <generator class="native"/>
      </id>
      
      <set name = "certificates" cascade="all">
         <key column = "employee_id"/>
         <one-to-many class="Certificate"/>
      </set>
      
      <property name = "firstName" column = "first_name" type = "string"/>
      <property name = "lastName" column = "last_name" type = "string"/>
      <property name = "salary" column = "salary" type = "int"/>
      
   </class>

   <class name = "Certificate" table = "CERTIFICATE">
      
      <meta attribute = "class-description">
         This class contains the certificate records. 
      </meta>
      
      <id name = "id" type = "int" column = "id">
         <generator class="native"/>
      </id>
      
      <property name = "name" column = "certificate_name" type = "string"/>
      
   </class>

</hibernate-mapping>
``` 


