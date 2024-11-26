### Connecting Java to a Database

To connect Java to MySQL using JDBC, assuming you have already created a table in MySQL, follow these 4 steps:

- Download the file mysql-connector-java-x.y.zz.zip from here, then extract the file to get mysql-connector-java-x.y.zz-bin.jar.
- Add the JDBC Driver library mysql-connector-java-x.y.zz-bin.jar to your project.
- Call the method Class.forName("com.mysql.jdbc.Driver").
- Call the method DriverManager.getConnection() to connect to the MySQL database.

Details of connecting a Java application to a MySQL database using JDBC are demonstrated in the example below.

Other databases can be connected in a similar way by changing the JDBC URL, username, and password.

