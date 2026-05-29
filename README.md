# BMW Website Clone

A full-stack BMW website clone built with a static HTML/CSS/JavaScript frontend and a Java Spring Boot backend.

This project supports user authentication, session handling, test drive bookings, contact form storage, and a user dashboard.

> This is an educational clone project and is not affiliated with BMW.

## Tech Stack

**Frontend**

- HTML
- CSS
- JavaScript

**Backend**

- Java 8
- Spring Boot 2.7
- Spring Web
- Spring JDBC
- BCrypt password hashing
- HTTP session based authentication
- SQLite

## Features

- User signup
- User login
- User logout
- BCrypt password hashing
- Session based authentication
- Test drive booking
- User booking dashboard
- Contact form submission storage
- Existing frontend served by Spring Boot

## Project Structure

```text
bmw/
|-- backend-java/
|   |-- data/
|   |   `-- bmw.sqlite
|   |-- src/main/java/com/bmwclone/
|   |   |-- BmwBackendApplication.java
|   |   |-- config/
|   |   |-- controller/
|   |   |-- dto/
|   |   `-- service/
|   |-- src/main/resources/
|   |   `-- application.properties
|   |-- pom.xml
|   `-- README.md
|-- frontend/
|   |-- bmw.html
|   |-- login.html
|   |-- signup.html
|   |-- dashboard.html
|   |-- contact.html
|   `-- images and static assets
`-- README.md
```

## Backend API

| Method | Endpoint | Description |
| --- | --- | --- |
| `POST` | `/api/auth/signup` | Create account and start session |
| `POST` | `/api/auth/login` | Login and start session |
| `POST` | `/api/auth/logout` | Logout and clear session |
| `GET` | `/api/auth/me` | Get current logged-in user |
| `POST` | `/api/bookings` | Create test drive booking |
| `GET` | `/api/bookings` | List current user's bookings |
| `POST` | `/api/contact` | Save contact form submission |

## Requirements

- Java 8 JDK
- Maven 3.8+

If Maven or JDK are not installed globally, this project can also be run with the local portable tools inside `backend-java/tools/` if they exist on your machine.

## Run The Project

From the project root:

```powershell
cd backend-java
mvn spring-boot:run
```

Open the website:

```text
http://localhost:8080/bmw.html
```

The frontend uses relative API paths like `/api/auth/login`, so no frontend API URL changes are needed when Spring Boot serves the frontend.

## Local Portable Run Command

If you are using the portable tools created locally in this project:

```powershell
cd "C:\pumo fullstack\bmw\backend-java"
$env:JAVA_HOME="C:\pumo fullstack\bmw\backend-java\tools\jdk8u492-b09"
$env:Path="$env:JAVA_HOME\bin;$env:Path"
.\tools\apache-maven-3.9.9\bin\mvn.cmd "-Dmaven.repo.local=C:\maven-repo" spring-boot:run
```

## Configuration

Main config file:

```text
backend-java/src/main/resources/application.properties
```

Default values:

```properties
server.port=8080
spring.datasource.url=jdbc:sqlite:${BMW_DB_PATH:data/bmw.sqlite}
bmw.frontend.path=${BMW_FRONTEND_PATH:../frontend}
```

Environment overrides:

- `BMW_DB_PATH`: SQLite database path
- `BMW_FRONTEND_PATH`: frontend folder path

## Test APIs

Use a cookie jar so the login session is preserved.

```powershell
curl.exe -c cookies.txt -b cookies.txt -H "Content-Type: application/json" `
  -d "{\"name\":\"Test User\",\"email\":\"test@example.com\",\"password\":\"Pass1234\"}" `
  http://localhost:8080/api/auth/signup

curl.exe -c cookies.txt -b cookies.txt http://localhost:8080/api/auth/me

curl.exe -c cookies.txt -b cookies.txt -H "Content-Type: application/json" `
  -d "{\"model\":\"BMW M4\",\"preferred_date\":\"2026-06-15\",\"location\":\"Chennai\"}" `
  http://localhost:8080/api/bookings

curl.exe -c cookies.txt -b cookies.txt http://localhost:8080/api/bookings

curl.exe -H "Content-Type: application/json" `
  -d "{\"name\":\"Visitor\",\"phone\":\"1234567890\",\"whatsapp\":\"\",\"email\":\"visitor@example.com\",\"message\":\"I want details.\"}" `
  http://localhost:8080/api/contact

curl.exe -c cookies.txt -b cookies.txt -X POST http://localhost:8080/api/auth/logout
```

## Troubleshooting

**`mvn` is not recognized**

Install Maven and add it to `PATH`, or use the local portable Maven command shown above.

**No compiler is provided**

You are using a JRE instead of a JDK. Install a Java 8 JDK and set `JAVA_HOME` to the JDK folder.

**Port 8080 is already in use**

Stop the existing process or run on another port:

```powershell
mvn spring-boot:run -Dspring-boot.run.arguments="--server.port=8081"
```

## Notes For GitHub

Do not commit local build output, dependency caches, or portable tool binaries. The `.gitignore` file excludes common local-only folders such as `target/`, `.m2/`, and `backend-java/tools/`.
