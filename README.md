# Spring Boot Blog REST API

[![Ask DeepWiki](https://devin.ai/assets/askdeepwiki.png)](https://deepwiki.com/AnkitMetalKnight/springboot-blog-app)

This project is a complete RESTful API for a blog application developed using the Spring Boot framework. It provides endpoints for managing blog posts and their associated comments, featuring functionalities like CRUD operations, pagination, sorting, and basic security.

## Features

-   **Full CRUD Operations for Posts**: Create, Read, Update, and Delete blog posts.
-   **Full CRUD Operations for Comments**: Create, Read, Update, and Delete comments associated with specific posts.
-   **Pagination and Sorting**: Retrieve a paginated and sorted list of all posts.
-   **One-to-Many Relationship**: Manages the relationship between Posts and Comments.
-   **DTO Pattern**: Utilizes Data Transfer Objects (DTOs) to transfer data between the client and server.
-   **Custom Exception Handling**: Implements a global exception handler for REST APIs.
-   **Validation**: Applies validation to request payloads using Jakarta Bean Validation (Hibernate Validator).
-   **Security**: Secures all API endpoints using Spring Security with HTTP Basic Authentication.

## Tech Stack

-   **Framework**: Spring Boot 3
-   **Language**: Java 17
-   **Database**: MySQL
-   **ORM**: Spring Data JPA / Hibernate
-   **Build Tool**: Maven
-   **Security**: Spring Security
-   **Utilities**:
    -   Lombok: To reduce boilerplate code.
    -   ModelMapper: For object mapping between DTOs and entities.

## Prerequisites

Before you begin, ensure you have the following installed on your system:

-   JDK 17 or later
-   Maven
-   MySQL Server

## Getting Started

Follow these steps to get the application running on your local machine.

### 1. Clone the Repository

```bash
git clone https://github.com/ankitmetalknight/springboot-blog-app.git
cd springboot-blog-app/springboot-blog-rest-api
```

### 2. Configure the Database

Open the `src/main/resources/application.properties` file and update the datasource and security properties with your local MySQL database and desired user credentials.

```properties
# Server Port
server.port=8070

# MySQL Datasource Configuration
spring.datasource.url=<YOUR_MYSQL_JDBC_URL>
spring.datasource.username=<YOUR_MYSQL_USERNAME>
spring.datasource.password=<YOUR_MYSQL_PASSWORD>

# Hibernate Configuration
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
spring.jpa.hibernate.ddl-auto=update

# Basic Authentication Credentials
spring.security.user.name=<YOUR_ADMIN_USERNAME>
spring.security.user.password=<YOUR_ADMIN_PASSWORD>
spring.security.user.roles=ADMIN
```
**Example `spring.datasource.url`**: `jdbc:mysql://localhost:3306/blog_db?createDatabaseIfNotExist=true`


### 3. Build and Run the Application

You can build and run the application using the Maven wrapper.

```bash
# Build the project
./mvnw clean install

# Run the application
./mvnw spring-boot:run
```

The application will start on port `8070` (or the port specified in `application.properties`).

## API Endpoints

All endpoints are secured with HTTP Basic Authentication. You will need to provide the configured username and password with each request.

### Post APIs

| Method | Endpoint                             | Description                                                              |
|--------|--------------------------------------|--------------------------------------------------------------------------|
| `POST` | `/api/posts`                         | Create a new blog post.                                                  |
| `GET`    | `/api/posts`                         | Get all posts with pagination and sorting.                               |
| `GET`    | `/api/posts/{id}`                    | Get a single post by its ID.                                             |
| `PUT`    | `/api/posts/{id}`                    | Update an existing post.                                                 |
| `DELETE` | `/api/posts/{id}`                    | Delete a post by its ID.                                                 |

#### Get All Posts (with Pagination & Sorting)

The `GET /api/posts` endpoint supports the following query parameters:
-   `pageNo`: Page number (default: `0`)
-   `pageSize`: Number of items per page (default: `10`)
-   `sortBy`: Field to sort by (default: `id`)
-   `sortDir`: Sort direction, `asc` or `desc` (default: `asc`)

**Example:** `GET /api/posts?pageNo=0&pageSize=5&sortBy=title&sortDir=asc`

### Comment APIs

| Method | Endpoint                                   | Description                                   |
|--------|--------------------------------------------|-----------------------------------------------|
| `POST` | `/api/posts/{postId}/comments`             | Create a new comment for a specific post.     |
| `GET`    | `/api/posts/{postId}/comments`             | Get all comments for a specific post.         |
| `GET`    | `/api/posts/{postId}/comments/{commentId}` | Get a single comment by its ID for a post.    |
| `PUT`    | `/api/posts/{postId}/comments/{commentId}` | Update an existing comment for a post.        |
| `DELETE` | `/api/posts/{postId}/comments/{commentId}` | Delete a comment by its ID for a post.        |

### Request/Response Examples

#### Create a Post

**Request:** `POST /api/posts`

**Body:**
```json
{
    "title": "My First Spring Boot Post",
    "description": "A short description of the post.",
    "content": "This is the full content of the blog post."
}
```

**Response:** `201 Created`
```json
{
    "id": 1,
    "title": "My First Spring Boot Post",
    "description": "A short description of the post.",
    "content": "This is the full content of the blog post.",
    "comments": []
}
```

#### Create a Comment

**Request:** `POST /api/posts/1/comments`

**Body:**
```json
{
    "name": "John Doe",
    "email": "john.doe@example.com",
    "body": "Great post! Thanks for sharing."
}
```

**Response:** `201 Created`
```json
{
    "id": 1,
    "name": "John Doe",
    "email": "john.doe@example.com",
    "body": "Great post! Thanks for sharing."
}
