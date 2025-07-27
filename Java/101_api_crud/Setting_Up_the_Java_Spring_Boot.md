# Setting Up the Java Spring Boot API with Docker and Maven

In this guide, we will set up a simple Spring Boot Java API with Maven, Docker, and MySQL as a backend. The guide will help you configure the project, build it with Maven, and run it in a Docker container.

## Steps

### 1. **Generate the Spring Boot Project**

Generate the project as a zip file from [Spring Initializr](https://start.spring.io/).

- Go to [https://start.spring.io/](https://start.spring.io/).
- Select the following options:
  - Project: Maven Project
  - Language: Java
  - Spring Boot: (choose the latest stable version)
  - Dependencies: Web, Data JPA, MySQL, Dockerfile
  - Packaging: Jar

- Click on **Generate** to download the zip file.

Once the zip file is downloaded, extract it into your desired project directory. For example:

```bash
unzip starter.zip -d my-java-api
cd my-java-api
```

### 2. **Configure MySQL Connection**  
Open `application.properties` or `application.yml` and set up the MySQL database:  

For **`application.properties`**:  
```
spring.datasource.url=jdbc:mysql://mysql:3306/mydb
spring.datasource.username=user
spring.datasource.password=password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```  

For **`application.yml`**:  
```yml
spring:
  datasource:
    url: jdbc:mysql://mysql:3306/mydb
    username: user
    password: password
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
```  

### 3. **Create a Docker Compose File**  
Create a `docker-compose.yml` file to run both the Java API and MySQL:  

```yml
version: "3.8"
services:
  mysql:
    image: mysql:8.0
    container_name: mysql_db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: mydb
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    ports:
      - "3306:3306"
  app:
    build: .
    container_name: java_api
    depends_on:
      - mysql
    ports:
      - "8080:8080"
    environment:
      SPRING_DATASOURCE_URL: jdbc:mysql://mysql:3306/mydb
      SPRING_DATASOURCE_USERNAME: user
      SPRING_DATASOURCE_PASSWORD: password
```  

---

### 4. **Verify and Install Dependencies with Maven**

Your project might be missing the necessary Spring Web dependency. Ensure that your `pom.xml` includes the following:

```xml
<dependencies>
    <!-- Other dependencies -->

    <!-- Spring Web Starter for RESTful APIs -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```
This will enable the `@RestController` and `@GetMapping` annotations used in your controller.

Before building the project, ensure that all dependencies are correctly installed. Run the following Maven command to install them:

```bash
mvn clean install
```

This command will:
- Clean any previous builds.
- Install required dependencies specified in your `pom.xml`.
- Build the project and generate a `target/demo-0.0.1-SNAPSHOT.jar` file.


### 5. **Create the `HelloWorld` Controller**

To test that your application is working, create a simple REST controller. Here's how:

1. Navigate to `src/main/java/com/example/demo/controller/`.
2. Create a new file called `HelloWorld.java`.

Inside `HelloWorld.java`, add the following code:

```java
package com.example.demo.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloWorld {

    @GetMapping("/hello")
    public String sayHello() {
        return "Hello, World!";
    }
}
```

This will create a basic controller that listens for GET requests at `/hello` and responds with "Hello, World!".

### 6. **Run the Application**

To run the application, use the following Maven command:

```bash
mvn spring-boot:run
```

You should see logs indicating that the application is starting up, and once it’s running, you can access the `/hello` endpoint at `http://localhost:8080/hello` to verify that it works.

### 7. **Build the Docker Image**

To containerize the application, you'll need to build a Docker image. Ensure your project includes a `Dockerfile` that looks like this:

```Dockerfile
FROM openjdk:17-jdk-slim
VOLUME /tmp
COPY target/demo-0.0.1-SNAPSHOT.jar demo.jar
ENTRYPOINT ["java", "-jar", "/demo.jar"]
```

Now, you can build the Docker image using the following command:

```bash
docker-compose up --build
```

Your Spring Boot API should now be available at `http://localhost:8080/hello`.

### 8. **Conclusion**

You now have a working Java API with Spring Boot, built using Maven, and containerized with Docker. You can extend this setup by adding more controllers, services, and connecting to a MySQL database as needed.
