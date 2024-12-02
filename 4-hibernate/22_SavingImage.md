### Saving Image

Often we need to save images or large documents in database. Databases provide saving characters or image files as BLOB (Binary Large Object) or CLOB (Character Large Object). In database, the type is BLOB/LONGBLOB, LONGTEXT etc.

To indicate that an entity has a field that has to be stored as a BLOB,CLOB the @Lob annotation is used. For example, consider the following code snippet −

```
@Entity
@Table(name = "save_image")
public class LobEntity {

...
   @Lob
   @Column(name = "image") 
   private byte[] image;
...
}
```

1. **Create Mapping Class**:

LobEntity.java

```java
package com.tutorialspoint;

import javax.persistence.*;

@Entity
@Table(name = "save_image")
public class LobEntity {

   @Id
   @GeneratedValue(strategy = GenerationType.IDENTITY) 
   private Long id; 

   public Long getId() {
      return id;
   }

   public void setId(Long id) {
      this.id = id;
   }

   @Lob
   @Column(name = "image") 
   private byte[] image;

   public byte [] getImage() {
      return image;
   }
   public void setImage(byte[] imageData) {
      this.image = imageData;
   }
}
```

2. **Create Hibernate Configuration File**:

hibernate.cfg.xml

```
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE hibernate-configuration PUBLIC  
   "-//Hibernate/Hibernate Configuration DTD 5.3//EN"  
   "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">  
<hibernate-configuration>  
   <session-factory>  
      <property name="hbm2ddl.auto">update</property>  
      <property name="dialect">org.hibernate.dialect.MySQL8Dialect</property>  
      <property name="connection.url">jdbc:mysql://localhost/TUTORIALSPOINT</property>  
      <property name="connection.username">root</property>  
      <property name="connection.password">guest123</property>  
      <property name="connection.driver_class">com.mysql.cj.jdbc.Driver</property>  
      <mapping class="com.tutorialspoint.LobEntity"/>  
   </session-factory>  
</hibernate-configuration> 
```

3. **Create Application Class**:

```java
package com.tutorialspoint;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.boot.Metadata;
import org.hibernate.boot.MetadataSources;
import org.hibernate.boot.registry.StandardServiceRegistry;
import org.hibernate.boot.registry.StandardServiceRegistryBuilder;

import java.io.*;

public class SaveBlob {
   public static void main(String[] args) throws FileNotFoundException, IOException {

      FileInputStream fis = new FileInputStream("C:\\Users\\Saikat\\OneDrive\\Pictures\\almoural_castle.jpg");
      byte[] bytes = new byte[fis.available()];
      StandardServiceRegistry ssr = new StandardServiceRegistryBuilder().configure("hibernate.cfg.xml").build();  
      Metadata meta = new MetadataSources(ssr).getMetadataBuilder().build();  
      SessionFactory factory = meta.getSessionFactoryBuilder().build();  
      Session session = factory.openSession();  
      Transaction t = session.beginTransaction();   
      LobEntity lob = new LobEntity();
      lob.setImage(bytes);
      session.save(lob);
      session.getTransaction().commit();
      fis.close();
      System.out.println("Successfully inserted image in table.");
   }
}
```





