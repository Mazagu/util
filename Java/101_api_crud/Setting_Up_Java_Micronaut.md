# 🚀 Setting Up a Java Micronaut API with Docker and Maven

Micronaut is a modern, JVM-based framework optimized for building microservices. Here's how to quickly set up a Micronaut project with Maven and Docker for local development and containerized deployment.

---

## 🛠 Prerequisites

- Java 17+
- Maven 3.8+
- Docker
- SDKMAN (optional, to install Micronaut CLI)

---

## ⚙️ 1. Create a Micronaut App (Maven)

You can generate the project using Micronaut CLI:

```
mn create-app com.example.hellomicronaut \
  --build maven \
  --lang java \
  --features http-server,graalvm,logback \
  --jdk 17
```

Or via [Micronaut Launch](https://micronaut.io/launch/)

---

## 📁 2. Project Structure Overview

```
hellomicronaut/
├── src/
│ └── main/java/com/example/hellomicronaut/
│ └── Controller.java
├── pom.xml
├── Dockerfile
└── application.yml
```

---

## 🧱 3. Basic Controller Example

Create a controller:

📄 `src/main/java/com/example/hellomicronaut/HelloController.java`

```
package com.example.hellomicronaut;

import io.micronaut.http.annotation.*;

@Controller("/hello")
public class HelloController {
    @Get("/")
    public String index() {
        return "Hello Micronaut!";
    }
}
```

---

## ⚙️ 4. Run Locally

Use Maven to run the app locally:

```
./mvnw mn:run
```

Visit: `http://localhost:8080/hello`

---

## 🐳 5. Dockerize the App

📄 `Dockerfile`

```
FROM eclipse-temurin:17-jdk as builder
WORKDIR /app
COPY . .
RUN ./mvnw package -DskipTests

FROM eclipse-temurin:17-jdk
WORKDIR /app
COPY --from=builder /app/target/hellomicronaut-*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

---

## 🔨 6. Build and Run Docker Image

```
docker build -t micronaut-api .
docker run -p 8080:8080 micronaut-api
```

Access your API at `http://localhost:8080/hello`

---

## ⚙️ 7. Configuration (Optional)

📄 `src/main/resources/application.yml`

```
micronaut:
  application:
    name: hellomicronaut
  server:
    port: 8080
```

---

## ✅ 8. Test Endpoint with curl

```
curl http://localhost:8080/hello
```

Expected output:

```
Hello Micronaut!
```

---

## 📦 9. Tips for Production

- Use `micronaut-runtime` and enable GraalVM native image support if needed.
- Mount configuration via Docker volumes or environment variables.
- Use `micronaut-openapi` for Swagger generation.
- Use `micronaut-security` for JWT/OAuth support.

---

## 📚 References

- [Micronaut Documentation](https://docs.micronaut.io/latest/guide/)
- [Micronaut Launch Generator](https://micronaut.io/launch/)
- [Micronaut Docker Support](https://guides.micronaut.io/latest/micronaut-docker-gradle-java.html)
