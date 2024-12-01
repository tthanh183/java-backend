### SortedMap Mapping

A SortedMap is a similar java collection as Map that stores elements in key-value pairs and provides a total ordering on its keys. Duplicate elements are not allowed in the map. The map is ordered according to the natural ordering of its keys, or by a Comparator typically provided at sorted map creation time.

A SortedMap is mapped with a <map> element in the mapping table and an ordered map can be initialized with java.util.TreeMap.

1. **Define RDBMS Tables**

```sql
create table EMPLOYEE (
   id INT NOT NULL auto_increment,
   first_name VARCHAR(20) default NULL,
   last_name  VARCHAR(20) default NULL,
   salary     INT  default NULL,
   PRIMARY KEY (id)
);
```

Further, assume each employee can have one or more certificate associated with him/her. We will store certificate related information in a separate table which has following structure −

```sql
create table CERTIFICATE (
   id INT NOT NULL auto_increment,
   certificate_type VARCHAR(40) default NULL,
   certificate_name VARCHAR(30) default NULL,
   employee_id INT default NULL,
   PRIMARY KEY (id)
);
```

There will be one-to-many relationship between EMPLOYEE and CERTIFICATE objects.

2. **Define POJO Classes**

```java
import java.util.*;

public class Employee {
   private int id;
   private String firstName; 
   private String lastName;   
   private int salary;
   private SortedMap certificates;

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

   public SortedMap getCertificates() {
      return certificates;
   }
   
   public void setCertificates( SortedMap certificates ) {
      this.certificates = certificates;
   }
}
```

We need to define another POJO class corresponding to CERTIFICATE table so that certificate objects can be stored and retrieved into the CERTIFICATE table. This class should also implement Comparable interface and compareTo method which will be used to sort the key elements of the SortedMap in case you set sort="natural" in your mapping file (see below mapping file).

```java
public class Certificate implements Comparable <String>{
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
   
   public int compareTo(String that){
      final int BEFORE = -1;
      final int AFTER = 1;

      if (that == null) {
         return BEFORE;
      }

      Comparable thisCertificate = this;
      Comparable thatCertificate = that;

      if(thisCertificate == null) {
         return AFTER;
      } else if(thatCertificate == null) {
         return BEFORE;
      } else {
         return thisCertificate.compareTo(thatCertificate);
      }
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
      
      <map name = "certificates" cascade="all" sort="MyClass">
         <key column = "employee_id"/>
         <index column = "certificate_type" type = "string"/>
         <one-to-many class="Certificate"/>
      </map>
      
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


> [!NOTE]
> - The mapping document is an XML document having `<hibernate-mapping>` as the root element which contains two <class> elements corresponding to each class.
> - The `<class>` elements are used to define specific mappings from a Java classes to the database tables. The Java class name is specified using the `name` attribute of the class element and the database table name is specified using the `table` attribute.
> - The `<meta>` element is optional element and can be used to create the class description.
> - The `<id>` element maps the unique ID attribute in class to the primary key of the database table. The `name` attribute of the id element refers to the property in the class and the `column` attribute refers to the column in the database table. The `type` attribute holds the hibernate mapping type, this mapping types will convert from Java to SQL data type.
> - The `<generator>` element within the id element is used to generate the primary key values automatically. The `class` attribute of the generator element is set to `native` to let hibernate pick up either `identity`, `sequence`, or `hilo` algorithm to create primary key depending upon the capabilities of the underlying database.
> - The `<property>` element is used to map a Java class property to a column in the database table. The `name` attribute of the element refers to the property in the class and the `column` attribute refers to the column in the database table. The `type` attribute holds the hibernate mapping type, this mapping types will convert from Java to SQL data type.
>   - The `<map>` element is used to set the relationship between Certificate and Employee classes. We used the `cascade` attribute in the <map> element to tell Hibernate to persist the Certificate objects at the same time as the Employee objects. The `name` attribute is set to the defined `SortedMap` variable in the parent class, in our case it is certificates. The `sort` attribute can be set to `natural` to have natural sorting or it can be set to a custom class implementing `java.util.Comparator`. We have used a class `MyClass` which implements java.util.Comparator to reverse the sorting order implemented in `Certificate` class.
> - The `<index>` element is used to represents the key parts of the key/value map pair. The key will be stored in the column certificate_type using a type of string.
> - The `<key>` element is the column in the CERTIFICATE table that holds the foreign key to the parent object i.e. table EMPLOYEE.
> - The `<one-to-many>` element indicates that one Employee object relates to many Certificate objects and, as such, the Certificate object must have an Employee parent associated with it. You can use either `<one-to-one>`, `<many-to-one>` or `<many-to-many>` elements based on your requirement.

If we use sort="natural" setting then we do not need to create a separate class because Certificate class already has implemented Comparable interface and hibernate will use compareTo() method defined in Certificate class to compare SortedMap keys. But we are using a custom comparator class MyClass in our mapping file so we would have to create this class based on our sorting algorithm. Let us do descending sorting of the keys available in the map.

```java
import java.util.Comparator;

public class MyClass implements Comparator <String>{
   public int compare(String o1, String o2) {
      final int BEFORE = -1;
      final int AFTER = 1;

      /* To reverse the sorting order, multiple by -1 */
      if (o2 == null) {
         return BEFORE * -1;
      }

      Comparable thisCertificate = o1;
      Comparable thatCertificate = o2;

      if(thisCertificate == null) {
         return AFTER * 1;
      } else if(thatCertificate == null) {
         return BEFORE * -1;
      } else {
         return thisCertificate.compareTo(thatCertificate) * -1;
      }
   }
}
    
```

4. **Create Application Class**

```java
import java.util.*;
 
import org.hibernate.HibernateException; 
import org.hibernate.Session; 
import org.hibernate.Transaction;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class ManageEmployee {
   private static SessionFactory factory; 
   public static void main(String[] args) {
      
      try{
         factory = new Configuration().configure().buildSessionFactory();
      }catch (Throwable ex) { 
         System.err.println("Failed to create sessionFactory object." + ex);
         throw new ExceptionInInitializerError(ex); 
      }
      
      ManageEmployee ME = new ManageEmployee();
      
      /* Let us have a set of certificates for the first employee  */
      TreeMap set1 = new TreeMap();
      set1.put("ComputerScience", new Certificate("MCA"));
      set1.put("BusinessManagement", new Certificate("MBA"));
      set1.put("ProjectManagement", new Certificate("PMP"));
  
      /* Add employee records in the database */
      Integer empID1 = ME.addEmployee("Manoj", "Kumar", 4000, set1);

      /* Another set of certificates for the second employee  */
      TreeMap set2 = new TreeMap();
      set2.put("ComputerScience", new Certificate("MCA"));
      set2.put("BusinessManagement", new Certificate("MBA"));

      /* Add another employee record in the database */
      Integer empID2 = ME.addEmployee("Dilip", "Kumar", 3000, set2);

      /* List down all the employees */
      ME.listEmployees();

      /* Update employee's salary records */
      ME.updateEmployee(empID1, 5000);

      /* Delete an employee from the database */
      ME.deleteEmployee(empID2);

      /* List down all the employees */
      ME.listEmployees();

   }

   /* Method to add an employee record in the database */
   public Integer addEmployee(String fname, String lname, int salary, TreeMap cert){
      Session session = factory.openSession();
      Transaction tx = null;
      Integer employeeID = null;
      
      try{
         tx = session.beginTransaction();
         Employee employee = new Employee(fname, lname, salary);
         employee.setCertificates(cert);
         employeeID = (Integer) session.save(employee); 
         tx.commit();
      } catch (HibernateException e) {
         if (tx!=null) tx.rollback();
         e.printStackTrace(); 
      } finally {
         session.close(); 
      }
      return employeeID;
   }

   /* Method to list all the employees detail */
   public void listEmployees( ){
      Session session = factory.openSession();
      Transaction tx = null;
      
      try {
         tx = session.beginTransaction();
         List employees = session.createQuery("FROM Employee").list(); 
         for (Iterator iterator1 = employees.iterator(); iterator1.hasNext();){
            Employee employee = (Employee) iterator1.next(); 
            System.out.print("First Name: " + employee.getFirstName()); 
            System.out.print("  Last Name: " + employee.getLastName()); 
            System.out.println("  Salary: " + employee.getSalary());
            SortedMap<String, Certificate> map = employee.getCertificates();
            for(Map.Entry<String,Certificate> entry : map.entrySet()){
               System.out.print("\tCertificate Type: " +  entry.getKey());
               System.out.println(",  Name: " + (entry.getValue()).getName());
            }
         }
         tx.commit();
      } catch (HibernateException e) {
         if (tx!=null) tx.rollback();
         e.printStackTrace(); 
      } finally {
         session.close(); 
      }
   }
   
   /* Method to update salary for an employee */
   public void updateEmployee(Integer EmployeeID, int salary ){
      Session session = factory.openSession();
      Transaction tx = null;
      
      try {
         tx = session.beginTransaction();
         Employee employee = (Employee)session.get(Employee.class, EmployeeID); 
         employee.setSalary( salary );
         session.update(employee);
         tx.commit();
      } catch (HibernateException e) {
         if (tx!=null) tx.rollback();
         e.printStackTrace(); 
      } finally {
         session.close(); 
      }
   }
   
   /* Method to delete an employee from the records */
   public void deleteEmployee(Integer EmployeeID){
      Session session = factory.openSession();
      Transaction tx = null;
      
      try {
         tx = session.beginTransaction();
         Employee employee = (Employee)session.get(Employee.class, EmployeeID); 
         session.delete(employee); 
         tx.commit();
      } catch (HibernateException e) {
         if (tx!=null) tx.rollback();
         e.printStackTrace(); 
      } finally {
         session.close(); 
      }
   }
}
```