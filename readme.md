📝 Notes App (React + Spring Boot)

A full-stack Notes Application built with React (frontend) and Spring Boot (backend).
Supports JWT authentication and CRUD operations on notes.

🚀 Features

🔐 User Authentication (JWT) – Signup & Login

🗒️ Notes CRUD – Create, Read, Update, Delete notes

👤 Notes linked to individual users

⚡ Optimistic Locking to handle conflicts

🛠️ Tech Stack

Frontend: React, Axios, React Router

Backend: Spring Boot, Spring Security, JPA/Hibernate

Database: MySQL (via XAMPP)

📌 Setup Instructions
1️⃣ Backend (Spring Boot)

Install Java 17+ and Maven

Clone backend project:

git clone <repo_url>
cd notes-app-backend


Configure application.properties:

spring.datasource.url=jdbc:mysql://localhost:3306/notesdb
spring.datasource.username=root
spring.datasource.password=
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect


Create DB:

CREATE DATABASE notesdb;


Run backend:

mvn spring-boot:run


→ Runs at http://localhost:8080

2️⃣ Frontend (React)

Create React app:

npx create-react-app notes-app-frontend
cd notes-app-frontend


Install dependencies:

npm install axios react-router-dom


Run frontend:

npm start


→ Runs at http://localhost:3000

🔗 API Routes
Auth
Method	Path	Description
POST	/auth/signup	Register user
POST	/auth/login	Login & get JWT
Notes (JWT required)
Method	Path	Description
POST	/notes	Create note
GET	/notes	Get all notes
GET	/notes/{id}	Get note by ID
PUT	/notes/{id}	Update note
DELETE	/notes/{id}	Delete note
📩 Example Requests

Create Note
➡️ Request:

{
  "title": "Shopping List",
  "content": "Milk, Eggs, Bread"
}


⬅️ Response:

{
  "id": 1,
  "title": "Shopping List",
  "content": "Milk, Eggs, Bread",
  "user_id": 5
}

🗄️ Database Schema
CREATE TABLE users (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE notes (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    user_id BIGINT NOT NULL,
    title VARCHAR(100) NOT NULL,
    content TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    version BIGINT,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

🔐 Authentication Choice

We use JWT (JSON Web Token) because:
✔️ Stateless – no session storage
✔️ Easy integration with React
✔️ Supported by Spring Security

⚠️ Failure Mode & Mitigation

Problem: Two users update the same note → last update overwrites changes.

Mitigation:

Use Optimistic Locking (@Version in notes table).

On version mismatch → API returns 409 Conflict:

{ "error": "Note was updated by another user, please refresh before saving." }

