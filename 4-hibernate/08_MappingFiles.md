### Mapping Files

An Object/relational mappings are usually defined in an XML document. This mapping file instructs Hibernate — how to map the defined class or classes to the database tables?

Though many Hibernate users choose to write the XML by hand, but a number of tools exist to generate the mapping document. These include XDoclet, Middlegen and AndroMDA for the advanced Hibernate users.

Let us consider our previously defined POJO class whose objects will persist in the table defined in next section.

```java
public class Employee {
   private int id;
   private String firstName; 
   private String lastName;   
   private int salary;  

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
}
```

There would be one table corresponding to each object you are willing to provide persistence. Consider above objects need to be stored and retrieved into the following RDBMS table −
    
```sql
CREATE TABLE `employee` (
   `id` int(11) NOT NULL auto_increment,
   `first_name` varchar(10) default NULL,
   `last_name` varchar(10) default NULL,
   `salary` int(11) default NULL,
   PRIMARY KEY  (`id`)
)
```

