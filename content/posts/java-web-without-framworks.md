---
title: "Java web without frameworks"
description: "como instalar android tools"
author:
  name: "Bruno Rozendo"
date: 2025-04-27
draft: false
tags:
- "java"
- "Jakarta EE"
- "tomcat"
comments: true
imageCredit: "<a href='http://www.freepik.com'>Designed by pikisuperstar / Freepik</a>"
image: "/images/posts/android-sdk.svg"
---
# Java Web, Before All the Frameworks

This blog post is designed for new developers who are just starting their journey in web development with Java. We'll explore how to build a web application using Jakarta EE (formerly Java EE) without relying on modern frameworks like Spring or Quarkus. Understanding these fundamentals will give you a solid foundation for working with any Java web technology.

## 1. Project Creation

Creating a Java web application starts with setting up a project structure. In this example, we're using Gradle as our build tool, but you could also use Maven or even a simple directory structure.

The first step is to create a new directory for your project and initialize it with Gradle. This can be done using the Gradle init command or by creating the necessary files manually.

Our project is a simple CRUD (Create, Read, Update, Delete) application that manages user data. It demonstrates the core components of a Java web application:

- Servlets for handling HTTP requests
- JSP (JavaServer Pages) for rendering HTML
- JDBC for database access
- Jakarta EE APIs for web development

## 2. Gradle Configuration

The `build.gradle` file is the heart of our project configuration. It defines the project dependencies, Java version, and build tasks.

```gradle
plugins {
  id 'war'
  id 'eclipse-wtp'
}

repositories {
  mavenCentral()
}

version = '1.0'

java{
    sourceCompatibility = JavaVersion.VERSION_21
    targetCompatibility = JavaVersion.VERSION_21
    toolchain {
        languageVersion = JavaLanguageVersion.of(21)
    }
}

dependencies {    
    providedRuntime "jakarta.el:jakarta.el-api:6.0.1"
    providedRuntime "jakarta.servlet:jakarta.servlet-api:6.1.0"
    providedRuntime "jakarta.servlet.jsp:jakarta.servlet.jsp-api:4.0.0"
    providedRuntime 'jakarta.websocket:jakarta.websocket-api:2.2.0'
    providedRuntime 'jakarta.authentication:jakarta.authentication-api:3.1.0'
    
    implementation  "jakarta.servlet.jsp.jstl:jakarta.servlet.jsp.jstl-api:3.0.2"
    implementation  "org.glassfish.web:jakarta.servlet.jsp.jstl:3.0.1"
}
```

Let's break down what this configuration does:

- The `war` plugin enables building a Web Application Archive (WAR) file, which is the standard format for deploying Java web applications.
- The `eclipse-wtp` plugin adds support for Eclipse Web Tools Platform, useful if you're using Eclipse as your IDE.
- We're using Java 21 for this project.
- The dependencies section includes:
   - Jakarta EE APIs for servlets, JSP, and other web components (marked as `providedRuntime` because the server provides them)
   - JSTL (JSP Standard Tag Library) for using tags in our JSP files

## 3. Project Structure

A typical Java web application follows a standard directory structure:

```
src/
├── main/
│   ├── java/              # Java source files
│   │   └── com/
│   │       └── crud/
│   │           ├── Base.java
│   │           ├── IndexController.java
│   │           ├── SampleController.java
│   │           └── db/
│   │               ├── DatabaseConfig.java
│   │               ├── UserController.java
│   │               └── UserDAO.java
│   ├── resources/         # Resources like properties files
│   │   └── db/
│   │       └── init.sql
│   └── webapp/            # Web application files
│       └── WEB-INF/
│           ├── web.xml    # Deployment descriptor
│           ├── layout.jsp
│           ├── index.jsp
│           ├── users.jsp
│           ├── sample.jsp
│           └── error.jsp
build.gradle                # Gradle build file
settings.gradle             # Gradle settings
```

This structure separates our code into logical components:

- Java source files in `src/main/java`
- Resource files in `src/main/resources`
- Web files in `src/main/webapp`

The `WEB-INF` directory is special - files in this directory are not directly accessible by clients, providing security for sensitive files like JSPs that contain business logic.

## 4. Jakarta EE 11 and Tomcat

Jakarta EE (formerly Java EE) is a set of specifications that define the standard APIs for enterprise Java development. Tomcat is a servlet container that implements the Jakarta Servlet, JSP, and WebSocket specifications.

### The web.xml File

The `web.xml` file, also known as the deployment descriptor, is a crucial configuration file for Jakarta EE web applications. It defines how the web server (like Tomcat) should handle requests, map servlets to URLs, and configure resources.

Here's our `web.xml` file:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd"
         version="6.0">
    <display-name>A Simple Application</display-name>

    <servlet>
        <servlet-name>IndexController</servlet-name>
        <servlet-class>com.crud.IndexController</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>IndexController</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>SampleController</servlet-name>
        <servlet-class>com.crud.SampleController</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>SampleController</servlet-name>
        <url-pattern>/sample</url-pattern>
    </servlet-mapping>
 
    <servlet>
        <servlet-name>UserController</servlet-name>
        <servlet-class>com.crud.db.UserController</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>UserController</servlet-name>
        <url-pattern>/users/*</url-pattern>
    </servlet-mapping>

    <!-- JNDI DataSource Reference -->
    <resource-ref>
        <description>PostgreSQL DataSource</description>
        <res-ref-name>jdbc/PostgresDB</res-ref-name>
        <res-type>javax.sql.DataSource</res-type>
        <res-auth>Container</res-auth>
    </resource-ref>
</web-app>
```

Let's break down the key elements:

- The root `<web-app>` element defines the Jakarta EE version (6.0) and XML namespaces.
- `<servlet>` elements define servlet classes and give them names.
- `<servlet-mapping>` elements map servlet names to URL patterns.
- `<resource-ref>` defines a reference to a resource, in this case, a JNDI DataSource for database connections.

The URL patterns determine which servlet handles which requests:
- `/` (root) is handled by IndexController
- `/sample` is handled by SampleController
- `/users/*` is handled by UserController (the `*` means it handles all paths starting with `/users/`)

This configuration allows the server to route incoming HTTP requests to the appropriate servlet based on the URL.

## 5. Servlets

Servlets are Java classes that handle HTTP requests and generate responses. They are the foundation of Java web applications, providing a way to process user input, interact with databases, and generate dynamic content.

Here's an example of a simple servlet from our project, the `IndexController`:

```java
package com.crud;

import java.util.ArrayList;

import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

public class IndexController extends Base {

    private static final long serialVersionUID = -6874811120813632685L;

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse res) {

        req.setAttribute("someVariable", "Um valor aqui");

        var list = new ArrayList<String>();
        list.add("item 1");
        list.add("item 2");

        req.setAttribute("someList", list);

        this.view("/WEB-INF/index.jsp", req, res);
    }
}
```

This servlet extends a base class and overrides the `doGet` method to handle HTTP GET requests. It:

1. Sets attributes in the request object (a string and a list)
2. Forwards the request to a JSP file for rendering

For more complex operations, like database access, we have the `UserController`:

```java
package com.crud.db;

import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import com.crud.Base;

import java.io.Serial;
import java.util.List;

public class UserController extends Base {

    private final UserDAO userDAO = new UserDAO();

    protected void doGet(HttpServletRequest req, HttpServletResponse res) {
        try {
            String pathInfo = req.getPathInfo();
            if (pathInfo == null || pathInfo.equals("/")) {
                // List all users
                List<String> users = userDAO.getAllUsers();
                req.setAttribute("users", users);
                req.setAttribute("pageTitle", "User List");
                this.view("/WEB-INF/users.jsp", req, res);
            } else {
                // Get single user
                String userId = pathInfo.substring(1);
                String user = userDAO.getUser(userId);
                if (user != null) {
                    res.setContentType("application/json");
                    res.getWriter().write(user);
                } else {
                    res.sendError(HttpServletResponse.SC_NOT_FOUND);
                }
            }
        } catch (Exception e) {
            req.setAttribute("error", "Error retrieving users: " + e.getMessage());
            this.view("/WEB-INF/error.jsp", req, res);
        }
    }

    // Other HTTP methods (doPost, doPut, doDelete) for CRUD operations...
}
```

This servlet handles different types of requests:
- GET requests to `/users` list all users
- GET requests to `/users/{id}` get a specific user
- It also has methods for POST, PUT, and DELETE to create, update, and delete users

The `Base` class provides common functionality for all servlets:

```java
package com.crud;

import jakarta.servlet.RequestDispatcher;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
import java.io.Serial;

public class Base extends HttpServlet {

    @Serial
    private static final long serialVersionUID = -6662457240855069965L;

    protected void view(String page, HttpServletRequest req, HttpServletResponse res) {
        try {
            res.setContentType("text/html; charset=utf-8");
            req.setAttribute("page", page);
            RequestDispatcher requestDispatcher = req.getRequestDispatcher("/WEB-INF/layout.jsp");
            requestDispatcher.forward(req, res);
        } catch (IOException | ServletException e) {
            res.setContentType("text/html; charset=utf-8");
            try (PrintWriter out = res.getWriter()) {
                out.println("<html><body>");
                out.println("<pre>");
                out.println(e.getLocalizedMessage());
                out.println("</pre>");
                out.println("</body></html>");
            } catch (IOException ex1) {
                ex1.printStackTrace();
            }
        }
    }
}
```

The `view` method sets up a simple template system, forwarding requests to a layout JSP that includes the specific page content.

## 6. JSP (JavaServer Pages)

JSP (JavaServer Pages) is a technology that allows you to create dynamic web pages using HTML with embedded Java code. JSPs are compiled into servlets by the server, combining the ease of writing HTML with the power of Java.

Our application uses a simple template system with a layout JSP and content JSPs:

### Layout JSP

The `layout.jsp` file provides a common structure for all pages:

```jsp
<%@ taglib uri="jakarta.tags.core"      prefix="c"   %>
<%@ taglib uri="jakarta.tags.fmt"       prefix="fmt" %>
<%@ taglib uri="jakarta.tags.xml"       prefix="x"   %>
<%@ taglib uri="jakarta.tags.sql"       prefix="sql" %>
<%@ taglib uri="jakarta.tags.functions" prefix="fn"  %>
<%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<c:import url="${page}" var="importedPage" />
<!doctype html>
<html lang="pt-BR">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title><c:out value="${title}" /></title>
    <!-- Latest compiled and minified CSS -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.4.1/dist/css/bootstrap.min.css" />
    <!-- Optional theme -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.4.1/dist/css/bootstrap-theme.min.css" />
    <!-- Latest compiled and minified JavaScript -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@3.4.1/dist/js/bootstrap.min.js"></script>

</head>
<body>
${importedPage}
</body>
</html>
```

This layout:
1. Imports JSTL tag libraries for use in the JSP
2. Imports the specific page content using `<c:import>`
3. Provides a common HTML structure with Bootstrap for styling
4. Inserts the imported page content into the body

### Content JSP

The `index.jsp` file is a simple content page:

```jsp
<%@ taglib uri="jakarta.tags.core"      prefix="c"   %>
<%@ taglib uri="jakarta.tags.fmt"       prefix="fmt" %>
<%@ taglib uri="jakarta.tags.xml"       prefix="x"   %>
<%@ taglib uri="jakarta.tags.sql"       prefix="sql" %>
<%@ taglib uri="jakarta.tags.functions" prefix="fn"  %>
<%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<c:set var="title" scope="request" value="Cadastrar"/>

<h3><c:out value="${someVariable}" /></h3>

<c:forEach var="li" items="${someList}">
        <p><c:out value="${li}" /></p>
</c:forEach>  
```

This JSP:
1. Sets a title for the page
2. Displays a variable value
3. Iterates through a list and displays each item

For more complex pages, like the users page, we use more JSTL features:

```jsp
<%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>

<div class="container">
    
    <c:if test="${not empty error}">
        <div class="alert alert-danger">
            ${error}
        </div>
    </c:if>
    
    <div class="row mb-4">
        <div class="col">
            <div class="card">
                <div class="card-header">
                    <h2>User List</h2>
                </div>
                <div class="card-body">
                    <c:choose>
                        <c:when test="${empty users}">
                            <p>No users found.</p>
                        </c:when>
                        <c:otherwise>
                            <ul class="list-group">
                                <c:forEach var="user" items="${users}">
                                    <li class="list-group-item">${user}</li>
                                </c:forEach>
                            </ul>
                        </c:otherwise>
                    </c:choose>
                </div>
            </div>
        </div>
    </div>
    
    <div class="row">
        <div class="col">
            <div class="card">
                <div class="card-header">
                    <h2>Add New User</h2>
                </div>
                <div class="card-body">
                    <form action="${pageContext.request.contextPath}/users" method="post">
                        <div class="form-group mb-3">
                            <label for="username">Username:</label>
                            <input type="text" class="form-control" id="username" name="username" required>
                        </div>
                        <div class="form-group mb-3">
                            <label for="email">Email:</label>
                            <input type="email" class="form-control" id="email" name="email" required>
                        </div>
                        <button type="submit" class="btn btn-primary">Add User</button>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
```

This JSP:
1. Displays an error message if one exists
2. Shows a list of users or a "No users found" message
3. Provides a form for adding new users

JSTL (JSP Standard Tag Library) provides tags for common tasks like conditionals (`<c:if>`, `<c:choose>`), loops (`<c:forEach>`), and output (`<c:out>`), making it easier to write dynamic content without embedding Java code directly.

## 7. Database, Data Retriever and Transactions

Our application uses JDBC (Java Database Connectivity) to interact with a PostgreSQL database. Let's explore how database connections are managed and how data is retrieved and modified.

### Database Configuration

The `DatabaseConfig` class manages database connections using JNDI (Java Naming and Directory Interface):

```java
package com.crud.db;

import javax.naming.Context;
import javax.naming.InitialContext;
import javax.naming.NamingException;
import javax.sql.DataSource;
import java.io.IOException;
import java.net.URISyntaxException;
import java.net.URL;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.Objects;

/**
 * Database configuration class that provides a DataSource for connecting to PostgreSQL.
 * Uses JNDI to lookup the DataSource configured in Tomcat.
 */
public class DatabaseConfig {
    private static DataSource dataSource;
    private static final String JNDI_NAME = "java:comp/env/jdbc/PostgresDB";

    static {
        try {
            initDataSource();
        } catch (Exception e) {
            throw new RuntimeException("Failed to lookup JNDI DataSource", e);
        }
    }

    private static void initDataSource() throws NamingException, SQLException {
        Context initContext = new InitialContext();
        dataSource = (DataSource) initContext.lookup(JNDI_NAME);

        if (dataSource == null) {
            throw new NamingException("Could not find JNDI DataSource: " + JNDI_NAME);
        }

        if (!isDatabaseIsInit(dataSource.getConnection())) {
            initDatabase(dataSource.getConnection());
        }
    }

    private static boolean isDatabaseIsInit(Connection conn) {
        try (PreparedStatement stmt = conn.prepareStatement("SELECT count(*) as c FROM users WHERE 1 =  1")) {
            ResultSet rs = stmt.executeQuery();
            rs.next();
            int count = rs.getInt("c");
            return count >= 0;
        } catch (SQLException e) {
            return false;
        }
    }

    private static void initDatabase(Connection conn){
        try {
            URL resource = DatabaseConfig.class.getClassLoader().getResource("db/init.sql");
            Path path = Path.of(Objects.requireNonNull(resource).toURI());
            String sqlScript = Files.readString(path);
            conn.prepareStatement(sqlScript).execute();
        } catch (SQLException | URISyntaxException | IOException e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * Get the configured DataSource
     * @return DataSource instance
     */
    public static DataSource getDataSource() {
        return dataSource;
    }

    /**
     * Get a database connection from the pool
     * @return Connection object
     * @throws SQLException if a database access error occurs
     */
    public static Connection getConnection() throws SQLException {
        return dataSource.getConnection();
    }

    /**
     * Close the data source and its connections
     * Note: With JNDI, the container manages the DataSource lifecycle,
     * so this method is now a no-op.
     */
    public static void closeDataSource() {
        // No need to close JNDI DataSource as it's managed by the container
    }
}
```

This class:
1. Looks up a DataSource from JNDI (Java Naming and Directory Interface)
2. Initializes the database if needed using an SQL script
3. Provides methods to get connections from the connection pool

### Database Connection Methods

There are several ways to create or acquire database connections in a Java web application:

1. **JNDI DataSource (as used in our project)**:
   - The application server (like Tomcat) manages the connection pool
   - The application looks up the DataSource using JNDI
   - Advantages: Connection pooling is handled by the server, configuration is separate from code

2. **Direct JDBC Connection**:
   ```java
   Connection conn = DriverManager.getConnection("jdbc:postgresql://localhost:5432/dbname", "username", "password");
   ```
   - Advantages: Simple, no dependencies
   - Disadvantages: No connection pooling, credentials in code

3. **Connection Pool Library (like HikariCP)**:
   ```java
   HikariConfig config = new HikariConfig();
   config.setJdbcUrl("jdbc:postgresql://localhost:5432/dbname");
   config.setUsername("username");
   config.setPassword("password");
   HikariDataSource dataSource = new HikariDataSource(config);
   Connection conn = dataSource.getConnection();
   ```
   - Advantages: Efficient connection pooling, configurable
   - Disadvantages: Additional dependency

4. **JPA/Hibernate**:
   ```java
   EntityManagerFactory emf = Persistence.createEntityManagerFactory("myPU");
   EntityManager em = emf.createEntityManager();
   // Use EntityManager for database operations
   ```
   - Advantages: Object-relational mapping, less SQL code
   - Disadvantages: More complex, additional dependencies

### context.xml Configuration

While our project doesn't include a `context.xml` file, it's an important file for configuring JNDI resources in Tomcat. Here's an example of what it would look like:

```xml
<Context>
   <Resource name="jdbc/PostgresDB"
             auth="Container"
             type="javax.sql.DataSource"
             driverClassName="org.postgresql.Driver"
             url="jdbc:postgresql://localhost:5432/your_database_name"
             username="your_username"
             password="your_password"
             maxTotal="20"
             maxIdle="10"
             maxWaitMillis="-1"/>
</Context>
```

This file would typically be placed in:
- `META-INF/context.xml` inside your WAR file
- `conf/[enginename]/[hostname]/[webappname].xml` in your Tomcat installation

### Data Access Object (DAO)

The `UserDAO` class handles database operations for users:

```java
package com.crud.db;

import java.sql.*;
import java.util.ArrayList;
import java.util.List;

public class UserDAO {
    
    public List<String> getAllUsers() throws Exception {
        List<String> users = new ArrayList<>();
        try (Connection conn = DatabaseConfig.getConnection();
             PreparedStatement stmt = conn.prepareStatement("SELECT * FROM users");
             ResultSet rs = stmt.executeQuery()) {
            
            while (rs.next()) {
                users.add(String.format("{\"id\":\"%s\",\"username\":\"%s\",\"email\":\"%s\"}",
                    rs.getString("id"),
                    rs.getString("username"),
                    rs.getString("email")));
            }
        }
        return users;
    }
    
    // Other CRUD methods...
}
```

This class:
1. Gets a connection from the connection pool
2. Executes SQL queries using prepared statements
3. Processes the results and returns them
4. Uses try-with-resources to ensure connections are properly closed

### Database Initialization

The `init.sql` file contains SQL statements to create tables and insert initial data:

```sql
-- Database initialization script

-- Create users table
CREATE TABLE IF NOT EXISTS users (
                                    id SERIAL PRIMARY KEY,
                                    username VARCHAR(50) NOT NULL UNIQUE,
   email VARCHAR(100) NOT NULL,
   created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );

-- Insert some sample data
INSERT INTO users (username, email) VALUES
                                       ('john_doe', 'john@example.com'),
                                       ('jane_smith', 'jane@example.com'),
                                       ('bob_johnson', 'bob@example.com')
   ON CONFLICT (username) DO NOTHING;
```

This script:
1. Creates a users table if it doesn't exist
2. Inserts sample data, skipping duplicates

## 8. Running

To run the application, you need to:

1. **Build the WAR file**:
   ```
   ./gradlew clean war
   ```

2. **Deploy to Tomcat**:
   - Copy the WAR file from `build/libs/servlet-jsp-1.0.war` to Tomcat's `webapps` directory
   - Start Tomcat: `bin/startup.sh` (Linux/Mac) or `bin\startup.bat` (Windows)

3. **Access the application**:
   - Open a browser and go to `http://localhost:8080/servlet-jsp/`
   - To see the user list, go to `http://localhost:8080/servlet-jsp/users`

4. **Docker for PostgreSQL** (optional):
   ```bash
   docker run --name postgres-db -e POSTGRES_PASSWORD=postgres -p 5432:5432 -d postgres:latest
   ```

## 9. Conclusions

### Pros of Understanding Traditional Java Web Development

1. **Foundational Knowledge**: Understanding servlets, JSP, and JDBC gives you a solid foundation for learning any Java web framework.

2. **Control**: You have direct control over every aspect of your application, from request handling to database access.

3. **Lightweight**: Without the overhead of frameworks, your application can be more lightweight and have fewer dependencies.

4. **Debugging**: When issues arise, it's easier to understand what's happening because you built the entire stack.

5. **Portability**: Servlet-based applications can run on any Jakarta EE-compliant server.

### Cons and Complexity Challenges

1. **Boilerplate Code**: You need to write a lot of repetitive code for common tasks.

2. **Manual Configuration**: Configuration is manual and can be error-prone.

3. **No Convention over Configuration**: Unlike modern frameworks, there are no defaults to speed up development.

4. **Limited Tooling**: Fewer development tools and utilities compared to popular frameworks.

5. **Maintenance Burden**: More code means more maintenance.

Modern frameworks like Spring, Jakarta EE with CDI, Quarkus, or Micronaut solve many of these problems by providing:
- Dependency injection
- Aspect-oriented programming
- Convention over configuration
- Rich ecosystems of libraries and tools

However, understanding how these frameworks work "under the hood" by learning the basics of servlets, JSP, and JDBC will make you a more effective Java developer, regardless of which framework you ultimately use.