# 📚 Book Management REST API

A simple RESTful API built with **Java**, **Spring Boot**, **MySQL**, **VS Code**, **Postman** to perform CRUD operations on books.

---

## 🔰 Project Goal

We'll build a simple **Book Management API** that supports:

* `GET /books` – list all books
* `POST /books` – add a new book
* `GET /books/{id}` – get a book by ID
* `PUT /books/{id}` – update a book
* `DELETE /books/{id}` – delete a book

### 📬 API Endpoints
| Method | Endpoint      | Description         |
| ------ | ------------- | ------------------- |
| GET    | `/books`      | Get all books       |
| GET    | `/books/{id}` | Get book by ID      |
| POST   | `/books`      | Create a new book   |
| PUT    | `/books/{id}` | Update a book by ID |
| DELETE | `/books/{id}` | Delete a book by ID |

---

## ✅ Prerequisites

Ensure the following are installed on your system:

- [Java JDK 17 or 21](https://adoptium.net/) - Make sure the option "JAVA_HOME environment variable" is checked during installation.
- [MySQL Server](https://dev.mysql.com/downloads/mysql/)
- [MySQL Workbench](https://dev.mysql.com/downloads/workbench/) (optional for DB GUI)
- [VS Code](https://code.visualstudio.com/) or any IDE
- [Postman](https://www.postman.com/downloads/) (for API testing)

---

## 🧩 Project Structure

```
bash
Copy
Edit
bookapi/
├── src/
│   └── main/
│       └── java/
│           └── com/example/bookapi/
│               ├── BookapiApplication.java   # Main class
│               ├── model/
│               │   └── Book.java             # Entity
│               ├── repository/
│               │   └── BookRepository.java   # JPA repository
│               └── controller/
│                   └── BookController.java   # REST controller
└── resources/
    └── application.properties                # DB config
```

---

## 🛡️ Notes

* Make sure your MySQL service is running before starting the app.
* If you're using VS Code, install the Java Extension Pack for better support.
* If JAVA_HOME is not set, you’ll need to configure it in system environment variables.

---

## 1. Project Setup

### Option A: Use Spring Initializer (recommended)

Visit [https://start.spring.io](https://start.spring.io) and select:

* Project: **Maven**
* Language: **Java**
* Spring Boot: **latest stable** - w/o SNAPSHOT
* Dependencies:

  * Spring Web
  * Spring Data JPA
  * MySQL Driver

Click **Generate** to download the zip --> unzip it --> open in VS Code.

### Option B: Create using terminal

```bash
curl https://start.spring.io/starter.zip \
-d dependencies=web,data-jpa,mysql \
-d name=bookapi \
-o bookapi.zip
unzip bookapi.zip
cd bookapi
code .
```

---

## 2. Configure MySQL

### Create a Database

Open MySQL Workbench and run:

```sql
CREATE DATABASE bookdb;
```

---

## 3. Configure `application.properties`

In `src/main/resources/application.properties`, update:

```properties
spring.application.name=bookapi
spring.datasource.url=jdbc:mysql://localhost:3306/bookdb
spring.datasource.username=root
spring.datasource.password=your_mysql_password
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

---

## 4. Define the Entity

Create a file: `src/main/java/com/example/bookapi/model/Book.java`

```java
package com.example.bookapi.model;

import jakarta.persistence.*;

@Entity
public class Book {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;
    private String author;

    public Book() {} // No-arg constructor

    // Getters & Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }

    public String getTitle() { return title; }
    public void setTitle(String title) { this.title = title; }

    public String getAuthor() { return author; }
    public void setAuthor(String author) { this.author = author; }
}
```

---

## 5. Create the Repository

Create a file: `src/main/java/com/example/bookapi/repository/BookRepository.java`:

```java
package com.example.bookapi.repository;

import com.example.bookapi.model.Book;
import org.springframework.data.jpa.repository.JpaRepository;

public interface BookRepository extends JpaRepository<Book, Long> {}
```

---

## 6. Create the Controller

Create a file: `src/main/java/com/example/bookapi/controller/BookController.java`:

```java
package com.example.bookapi.controller;

import com.example.bookapi.model.Book;
import com.example.bookapi.repository.BookRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/books")
public class BookController {

    @Autowired
    private BookRepository repo;

    @GetMapping
    public List<Book> getAllBooks() {
        return repo.findAll();
    }

    @PostMapping
    public Book createBook(@RequestBody Book book) {
        return repo.save(book);
    }

    @GetMapping("/{id}")
    public Book getBook(@PathVariable Long id) {
        return repo.findById(id).orElse(null);
    }

    @PutMapping("/{id}")
    public Book updateBook(@PathVariable Long id, @RequestBody Book bookDetails) {
        Book book = repo.findById(id).orElse(null);
        if (book == null) return null;
        book.setTitle(bookDetails.getTitle());
        book.setAuthor(bookDetails.getAuthor());
        return repo.save(book);
    }

    @DeleteMapping("/{id}")
    public void deleteBook(@PathVariable Long id) {
        repo.deleteById(id);
    }
}
```

---

## 7. Create the API Application

Create a file: `src\main\java\com\example\bookapi\BookapiApplication.java`:

```java
package com.example.bookapi;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class BookapiApplication {
    public static void main(String[] args) {
        SpringApplication.run(BookapiApplication.class, args);
    }
}
```

---

## 8. Run the App

In terminal:

```bash
./mvnw spring-boot:run
```

It should run on `http://localhost:8080`.

---

## 9. Test with Postman

Use Postman to make requests like:

* `GET http://localhost:8080/books`
* `POST http://localhost:8080/books`

🔸 For POST --> Body --> raw --> JSON:
```json
{
  "title": "The Pragmatic Programmer",
  "author": "Andy Hunt"
}
```
---

## 💡 Future Enhancements

* Add Swagger/OpenAPI documentation
* Add DTOs and service layer
* Include exception handling and logging
* Add pagination support

---

## 📄 License

This project is open-source and free to use.

---