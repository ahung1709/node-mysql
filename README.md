# Node.js Authentication System

Simple authentication system built while learning Node.js + MySQL.

Users can register, log in, and access a protected profile page using JWT authentication stored in cookies.

---

## Table of Contents

- [Quick Start - Run This Project in 60 Seconds](#quick-start---run-this-project-in-60-seconds)
- [Tech Stack](#tech-stack)
- [Features](#features)
- [Project Structure](#project-structure)
- [Routes](#routes)
- [API Request Examples](#api-request-examples)
- [Architecture](#architecture)
- [Environment Variables](#environment-variables)
- [Database Setup](#database-setup)
- [Database Schema](#database-schema)
- [Running the App](#running-the-app)
- [Quick Commands](#quick-commands)
- [Authentication Flow](#authentication-flow)
- [Status](#status)
- [Developer Notes](#developer-notes)
- [Development Checklist](#development-checklist)

---

## Quick Start - Run This Project in 60 Seconds

1. Start MySQL in MAMP
2. Install dependencies

   `npm install`

3. Start server

   `npm start`

4. Open

   `http://localhost:5001`

## Tech Stack

**Backend**

- Node.js
- Express

**Authentication**

- jsonwebtoken
- bcryptjs
- cookie-parser

**Database**

- MySQL
- phpMyAdmin

**Frontend**

- Handlebars (hbs)
- Bootstrap
- CSS

**Development Environment**

- MAMP (used to run MySQL)

---

## Features

- User registration with hashed passwords
- Login authentication
- JWT stored in cookies
- Protected routes using middleware
- Navigation links change depending on login status

When logged in:

- Hide **Login** and **Register**
- Show **Profile** and **Logout**

---

## Project Structure

```
project-root
│
├── controllers
│
├── routes
│   ├── auth
│   └── pages
├── views
│   ├── index
│   ├── login
│   ├── profile
│   └── register
│
├── public
│   ├── avatar
│   └── css
│
├── .env
├── package.json
└── app.js
```

---

## Routes

| Route            | Description            |
| ---------------- | ---------------------- |
| `/`              | Home page              |
| `/login`         | Login page             |
| `/register`      | Register page          |
| `/profile`       | Protected profile page |
| `/auth/register` | Register user          |
| `/auth/login`    | Login user             |
| `/auth/logout`   | Logout user            |

`/profile` is protected by authentication middleware.

## API Request Examples

### Register User

POST `/auth/register`

Body:

```json
{
  "name": "test user",
  "email": "test@email.com",
  "password": "123456",
  "passwordConfirm": "123456"
}
```

Expected behavior:

- Password is hashed with bcrypt
- User stored in MySQL
- 'User registered' or error message returned

### Login User

POST `/auth/login`

Body:

```json
{
  "email": "test@email.com",
  "password": "123456"
}
```

Expected behavior:

- Password compared using bcrypt
- JWT created
- JWT stored in cookie
- User redirected to home page

### Logout

GET `/auth/logout`

Expected behavior:

- JWT cookie cleared
- User redirected to home page

---

## Architecture

```

Browser
│
│ HTTP Request
▼
Express Server
│
├── Routes
│ /
│ /login
│ /register
│ /profile
│ /auth/register
│ /auth/login
│ /auth/logout
│
├── Controllers
│ authController.js
│
├── Middleware (protect routes)
│
▼
MySQL Database
│
└── users table

```

---

## Environment Variables

Create `.env` in the project root:

```

DATABASE=<database>
DATABASE_HOST=<database-host>
DATABASE_USER=<database-user>
DATABASE_PASSWORD=<database-password>
DATABASE_PORT=<database-port>

JWT_SECRET=<jwt-secret>
JWT_EXPIRES_IN=<jwt-expires-in>
JWT_COOKIE_EXPIRES=<jwt-cookie-expires>

```

---

## Database Setup

MySQL is run locally using **MAMP**.

Open phpMyAdmin:

```

http://localhost:8888/phpMyAdmin5/

```

Create a database and configure the connection values in `.env`.

## Database Schema

### users

| Column   | Type         | Notes                                     |
| -------- | ------------ | ----------------------------------------- |
| id       | INT          | primary key - auto increment              |
| name     | VARCHAR(100) | name                                      |
| email    | VARCHAR(100) | login credential - email                  |
| password | VARCHAR(255) | login credential - password (bcrypt hash) |

MySQL Database can be created either manually or using SQL inside of phpMyAdmin.

Example flow to create a database via phpMyAdmin:

```

Open phpMyAdmin → New → Type 'nodejs-login' for Database name → Create

```

Example flow to create a users table in the database 'nodejs-login' via phpMyAdmin:

```

Open phpMyAdmin → nodejs-login → New → Input the following details → Save

| Name     | Type    | Length | A_I         |
| -------- | ------- | ------ | ----------- |
| id       | INT     |        | Checked     |
| name     | VARCHAR | 100    | Not checked |
| email    | VARCHAR | 100    | Not checked |
| password | VARCHAR | 255    | Not checked |

```

Example SQL to create a users table in the database 'nodejs-login' via phpMyAdmin:

```

Open phpMyAdmin → nodejs-login → SQL → Input the following SQL → Go

CREATE TABLE users (
id INT AUTO_INCREMENT PRIMARY KEY,
name VARCHAR(100) NOT NULL,
email VARCHAR(100) NOT NULL,
password VARCHAR(255) NOT NULL
);

```

---

## Running the App

Install dependencies:

```

npm install

```

Start the dev server:

```

npm start

```

Open in browser:

```

http://localhost:5001

```

---

## Quick Commands

Install dependencies

```
npm install
```

Restart MySQL (MAMP)

```
Open MAMP → Start Servers
```

Start server

```
npm start
```

---

## Authentication Flow

1. User registers with email + password
2. Password hashed using bcrypt
3. User logs in
4. Server creates JWT
5. JWT stored in cookie
6. Middleware verifies token
7. Protected routes accessible when authenticated

---

## Status

Local development only.
Not deployed.

---

## Developer Notes

### Things Learned

- bcrypt hashes passwords before storing
- JWT stored in cookies for authentication
- middleware protects routes
- Handlebars conditionally renders navigation

### Implementation Notes

- JWT stored in cookies
- bcrypt used to hash passwords
- middleware protects `/profile` route
- navigation changes based on login status

## Development Checklist

### Authentication

- [x] User registration
- [x] Password hashing with bcrypt
- [x] User login
- [x] JWT authentication
- [x] Store JWT in cookie
- [x] Logout functionality
- [x] Protected `/profile` route

### UI

- [x] Home page
- [x] Login page
- [x] Register page
- [x] Profile page
- [x] Conditional navigation links

### Database

- [x] MySQL connection
- [x] Users table
- [ ] Add email uniqueness validation
- [ ] Add password reset support

### Future Improvements

- [ ] Email verification
- [ ] Password reset
- [ ] Role-based authorization
- [ ] API version of authentication
- [ ] Deploy application
