# Creating a **RESTful API using Java, Spring Boot, and MySQL**, using **VS Code**, **Postman**, and **MySQL Workbench**

---

## ✅ Project Goal

We'll build a simple **Book Management API** that supports:

* `GET /books` – list all books
* `POST /books` – add a new book
* `GET /books/{id}` – get a book by ID
* `PUT /books/{id}` – update a book
* `DELETE /books/{id}` – delete a book

---

## 1️⃣ Project Setup

### Option A: Use Spring Initializer (recommended)

Visit [https://start.spring.io](https://start.spring.io) and select:

* Project: Maven
* Language: Java
* Spring Boot: (latest stable)
* Dependencies:

  * Spring Web
  * Spring Data JPA
  * MySQL Driver

Click **Generate** to download the zip → unzip it → open in **VS Code**.

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

## 2️⃣ Configure MySQL

### Create a Database

Open MySQL Workbench and run:

```sql
CREATE DATABASE bookdb;
```

---

## 3️⃣ Configure `application.properties`

In `src/main/resources/application.properties`, add:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/bookdb
spring.datasource.username=root
spring.datasource.password=your_mysql_password
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

---

## 4️⃣ Define the Entity

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

    // Getters and setters
}
```

---

## 5️⃣ Create the Repository

Create `BookRepository.java`:

```java
package com.example.bookapi.repository;

import com.example.bookapi.model.Book;
import org.springframework.data.jpa.repository.JpaRepository;

public interface BookRepository extends JpaRepository<Book, Long> {}
```

---

## 6️⃣ Create the Controller

Create `BookController.java`:

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

## 7️⃣ Run the App

In terminal:

```bash
./mvnw spring-boot:run
```

It should run on `http://localhost:8080`.

---

## 8️⃣ Test with Postman

Use Postman to make requests like:

* `GET http://localhost:8080/books`
* `POST http://localhost:8080/books`

  ```json
  {
    "title": "Clean Code",
    "author": "Robert Martin"
  }
  ```

---

## ✅ Done!

Would you like me to:

* Help generate this as a GitHub repo?
* Add Swagger API docs?
* Add unit testing or Docker support?

Let me know how far you want to go.
