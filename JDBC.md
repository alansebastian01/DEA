# JDBC (Java Database Connectivity) – Study Notes

## Definition

**JDBC (Java Database Connectivity)** is a Java API that allows Java applications to connect to databases, execute SQL queries, and retrieve or update data.

**In simple terms:**
JDBC acts as a **bridge between a Java program and a database**.

---

# Why Use JDBC?

JDBC allows Java programs to:

* Connect to a database
* Execute SQL statements
* Retrieve data
* Insert, update, and delete records
* Manage database transactions

---

# JDBC Architecture

```
Java Application
       │
       ▼
   JDBC API
       │
       ▼
 JDBC Driver Manager
       │
       ▼
 JDBC Driver
       │
       ▼
 Database (MySQL, Oracle, PostgreSQL, etc.)
```

---

# Components of JDBC

### 1. JDBC API

Provides classes and interfaces for database operations.

Examples:

* Connection
* Statement
* PreparedStatement
* ResultSet

---

### 2. JDBC Driver

Software that enables communication between Java and the database.

Different databases have different JDBC drivers.

Examples:

* MySQL Connector/J
* Oracle JDBC Driver
* PostgreSQL JDBC Driver

---

### 3. DriverManager

Manages JDBC drivers and establishes database connections.

Example:

```java
Connection con = DriverManager.getConnection(url, user, password);
```

---

### 4. Connection

Represents a connection between the Java application and the database.

Used to:

* Create SQL statements
* Commit transactions
* Close the connection

---

### 5. Statement

Executes simple SQL queries.

Example:

```java
Statement stmt = con.createStatement();
```

---

### 6. PreparedStatement

Executes **parameterized SQL queries**.

Benefits:

* Faster execution
* Prevents SQL injection
* Reusable

Example:

```java
PreparedStatement ps =
con.prepareStatement("SELECT * FROM Student WHERE id=?");
ps.setInt(1, 101);
```

---

### 7. ResultSet

Stores the data returned from a SELECT query.

Example:

```java
ResultSet rs = stmt.executeQuery("SELECT * FROM Student");
```

Access data:

```java
while(rs.next()){
    System.out.println(rs.getString("name"));
}
```

---

# Steps to Connect JDBC to a Database

1. Import JDBC packages
2. Load the JDBC driver (optional in modern JDBC)
3. Establish a connection
4. Create a Statement or PreparedStatement
5. Execute SQL query
6. Process the ResultSet (if any)
7. Close resources

---

# Example JDBC Program

```java
import java.sql.*;

public class JDBCExample {
    public static void main(String[] args) throws Exception {

        String url = "jdbc:mysql://localhost:3306/college";
        String username = "root";
        String password = "root";

        Connection con = DriverManager.getConnection(url, username, password);

        Statement stmt = con.createStatement();

        ResultSet rs = stmt.executeQuery("SELECT * FROM Student");

        while(rs.next()) {
            System.out.println(rs.getInt("id") + " " +
                               rs.getString("name"));
        }

        rs.close();
        stmt.close();
        con.close();
    }
}
```

---

# Types of JDBC Drivers

| Type   | Name                    | Description                                      |
| ------ | ----------------------- | ------------------------------------------------ |
| Type 1 | JDBC-ODBC Bridge        | Uses ODBC; obsolete and removed from modern Java |
| Type 2 | Native API Driver       | Uses native database libraries                   |
| Type 3 | Network Protocol Driver | Uses middleware server                           |
| Type 4 | Thin Driver             | Pure Java driver; most commonly used             |

**Most commonly used:** **Type 4 (Thin Driver)**

---

# Statement vs PreparedStatement

| Statement                   | PreparedStatement          |
| --------------------------- | -------------------------- |
| Executes simple SQL         | Executes parameterized SQL |
| Slower                      | Faster (precompiled)       |
| Vulnerable to SQL injection | Prevents SQL injection     |
| Not reusable                | Reusable                   |

---

# Advantages of JDBC

* Platform independent
* Supports multiple databases
* Easy to execute SQL queries
* Enables CRUD operations (Create, Read, Update, Delete)
* Supports transactions
* Widely used in Java applications

---

# Disadvantages of JDBC

* Requires SQL knowledge
* More boilerplate code compared to ORM frameworks
* Manual resource management (connections, statements, result sets)
* Database-specific drivers are required

---

# Applications of JDBC

* Banking systems
* Student management systems
* E-commerce applications
* Inventory management
* Hospital management systems
* Web applications

---

# Quick Exam Summary

* **JDBC** = Java Database Connectivity
* Used to connect **Java applications** with **databases**
* Main classes/interfaces: **DriverManager, Connection, Statement, PreparedStatement, ResultSet**
* **PreparedStatement** is preferred because it is faster and prevents SQL injection.
* **Type 4 (Thin Driver)** is the most commonly used JDBC driver.
* Basic workflow: **Connect → Create Statement → Execute SQL → Process Result → Close Resources**
