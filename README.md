# 📚 Book Management REST API

A simple RESTful API built with **Java**, **Spring Boot**, **MySQL**, **VS Code**, **Postman** to perform CRUD operations on books.

GitHub Link : https://github.com/prphub/BookMgmt-SpringBoot-API

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

## 🛠️ Setup Instructions

## 1. Clone Project

```bash
git clone https://github.com/prphub/BookMgmt-SpringBoot-API.git
cd BookMgmt-SpringBoot-API
code .
```

---

## 2. Run the App

In terminal:

```bash
./mvnw spring-boot:run
```

It should run on `http://localhost:8080`.

---

## 3. Test with Postman

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