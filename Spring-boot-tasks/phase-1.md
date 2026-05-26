# Spring Boot Backend Learning Roadmap for Freshers

## Goal
Build a simple backend project using:
- Java
- Spring Boot
- MySQL

Duration:
- 2 Hours/Day
- 3 to 4 Weeks

---

# Phase 1 - Environment Setup & Basics

## Task 1 - Install Required Tools

Install the following tools:

- IntelliJ IDEA Community Edition
- Java (JDK 17 recommended)
- Maven
- Postman
- MySQL

### Verify Installation

Ensure the following commands work:

```bash
java -version
mvn -version
```

Also verify:
- IntelliJ opens properly
- MySQL service is running
- Postman is installed successfully

---

## Task 2 - Create First Spring Boot Project

### Steps

1. Create a Spring Boot project using Spring Initializr
2. Add only one dependency:
   - Spring Web
3. Project Name:
   - `demo-app`
4. Import the project into IntelliJ
5. Run the application

### Expected Result

Spring Boot application should start successfully on:

```text
http://localhost:8080
```

---

## Task 3 - Understand Project Structure

Understand the purpose of the following:

### Important Folders & Files

- `src/main/java`
- `src/main/resources`
- `pom.xml`
- `application.properties`

### Learn the Basics

- What Maven dependencies do
- Why `@SpringBootApplication` is used
- What happens when Spring Boot application starts

---

# Phase 2 - REST API Basics

## Task 4 - Create First Controller

### Requirements

1. Create `HelloController`
2. Create one GET API
3. Return simple text response

### Example

```java
@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello World";
    }
}
```

### Test Using

- Browser
- Postman

### Expected URL

```text
http://localhost:8080/hello
```

---

## Task 5 - Learn Different HTTP Methods

Create dummy APIs for the following HTTP methods:

- GET
- POST
- PUT
- DELETE

### Goal

Understand:
- Purpose of each HTTP method
- How APIs are created in Spring Boot

---

## Task 6 - Learn RequestBody

### Requirements

1. Create a Java class:
   - `Student`
2. Send JSON data from Postman
3. Return the same object in response

### Example JSON

```json
{
  "name": "Surya",
  "age": 22
}
```

### Goal

Understand:
- `@RequestBody`
- JSON to Java object mapping
- Request and Response flow

---

## Task 7 - Learn PathVariable & RequestParam

### Implement APIs Using

- `@PathVariable`
- `@RequestParam`

### Examples

```java
@GetMapping("/student/{id}")
```

```java
@GetMapping("/search")
```

### Goal

Understand:
- Dynamic URL values
- Query parameters

---

# Phase 3 - Layered Architecture

## Task 8 - Create Service Layer

### Implement Flow

```text
Controller -> Service
```

### Goal

Understand:
- Why business logic should not stay inside Controller
- Separation of concerns
- Clean architecture basics

---

## Task 9 - Return List of Objects

### Requirements

1. Create a list of objects
2. Return data from:
   - Service layer
   - Controller layer

### Example

```java
List<Student>
```

### Goal

Understand:
- Returning collections as JSON
- Layer-to-layer communication

---

# Phase 4 - Database Integration

## Task 10 - Install MySQL & Create Database

### Create Database

```sql
CREATE DATABASE student_db;
```

### Learn

- Database
- Table
- Row
- Primary Key

---

## Task 11 - Connect Spring Boot to MySQL

### Add Required Dependencies

- Spring Data JPA
- MySQL Driver

### Configure `application.properties`

Example:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/student_db
spring.datasource.username=root
spring.datasource.password=your_password
```

### Goal

Successfully connect Spring Boot application to MySQL database.

---

## Task 12 - Write DAO Layer

### Requirements

1. Create Entity class
2. Create Repository / DAO layer
3. Save data into database
4. Fetch data from database

### Goal

Understand:
- JPA
- Repository layer
- Database interaction

---

# Phase 5 - Complete CRUD Operations

## Task 13 - Implement Complete CRUD Operations

Implement the following APIs:

- Create Student
- Get All Students
- Get Student By ID
- Update Student
- Delete Student

### Goal

Understand complete backend flow:

```text
Controller -> Service -> DAO -> Database
```

---

# Final Outcome

By the end of this roadmap, the learner should be able to:

- Create Spring Boot applications
- Build REST APIs
- Use Controllers and Services
- Connect applications to MySQL
- Perform CRUD operations
- Test APIs using Postman
- Understand basic backend architecture

---

# Suggested Mini Project

Choose one simple project:

- Student Management System
- Employee Management System
- Book Management System
- Task Tracker API

Keep the project simple and focus on fundamentals.