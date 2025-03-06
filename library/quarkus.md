# Quarkus Cheat Sheet

## **1. Installation & Setup**  
### **Install Quarkus CLI (Recommended)**  
```bash
npm install -g @quarkusio/quarkus-cli
```
### **Manual Setup (Maven-based)**  
```bash
mvn io.quarkus.platform:quarkus-maven-plugin:3.0.0:create \
    -DprojectGroupId=com.example \
    -DprojectArtifactId=my-quarkus-app \
    -DclassName="com.example.GreetingResource" \
    -Dpath="/hello"
```
Navigate to the project directory:  
```bash
cd my-quarkus-app
```

---

## **2. Running & Building the Application**  
### **Run in Development Mode (Hot Reload)**  
```bash
./mvnw quarkus:dev
```
### **Build Native Executable (GraalVM Required)**  
```bash
./mvnw package -Pnative
```
### **Containerize with Quarkus Native**  
```bash
./mvnw package -Dquarkus.container-image.build=true
```

---

## **3. Basic REST API with JAX-RS**  
```java
package com.example;

import jakarta.ws.rs.GET;
import jakarta.ws.rs.Path;
import jakarta.ws.rs.Produces;
import jakarta.ws.rs.core.MediaType;

@Path("/hello")
public class GreetingResource {

    @GET
    @Produces(MediaType.TEXT_PLAIN)
    public String hello() {
        return "Hello, Quarkus!";
    }
}
```

---

## **4. Dependency Injection with CDI**  
```java
import jakarta.enterprise.context.ApplicationScoped;

@ApplicationScoped
public class GreetingService {
    public String greet() {
        return "Hello from CDI!";
    }
}
```
Injecting the Service:  
```java
import jakarta.inject.Inject;
import jakarta.ws.rs.GET;
import jakarta.ws.rs.Path;

@Path("/greet")
public class GreetingResource {

    @Inject
    GreetingService greetingService;

    @GET
    public String greet() {
        return greetingService.greet();
    }
}
```

---

## **5. Configuration (application.properties or YAML)**  
### **Using `application.properties`**  
```properties
quarkus.http.port=8081
greeting.message=Hello from Config!
```
### **Injecting Configuration in Java**  
```java
import org.eclipse.microprofile.config.inject.ConfigProperty;
import jakarta.inject.Inject;
import jakarta.ws.rs.GET;
import jakarta.ws.rs.Path;

@Path("/config")
public class ConfigResource {

    @Inject
    @ConfigProperty(name = "greeting.message")
    String message;

    @GET
    public String getMessage() {
        return message;
    }
}
```

---

## **6. Database Access with Hibernate & Panache**  
### **Add Dependency**
```xml
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-hibernate-orm-panache</artifactId>
</dependency>
```
### **Entity Definition**
```java
import jakarta.persistence.Entity;
import io.quarkus.hibernate.orm.panache.PanacheEntity;

@Entity
public class Person extends PanacheEntity {
    public String name;
}
```
### **Repository with Panache**  
```java
import jakarta.enterprise.context.ApplicationScoped;
import io.quarkus.hibernate.orm.panache.PanacheRepository;

@ApplicationScoped
public class PersonRepository implements PanacheRepository<Person> {
}
```
### **CRUD API with Panache**  
```java
import jakarta.inject.Inject;
import jakarta.ws.rs.*;

import java.util.List;

@Path("/persons")
@Produces("application/json")
@Consumes("application/json")
public class PersonResource {

    @Inject
    PersonRepository personRepository;

    @GET
    public List<Person> getAll() {
        return personRepository.listAll();
    }

    @POST
    public void addPerson(Person person) {
        personRepository.persist(person);
    }
}
```

---

## **7. REST Clients (MicroProfile REST Client)**  
### **Add Dependency**
```xml
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-rest-client</artifactId>
</dependency>
```
### **Define REST Client Interface**
```java
import org.eclipse.microprofile.rest.client.inject.RegisterRestClient;
import jakarta.ws.rs.GET;
import jakarta.ws.rs.Path;
import jakarta.ws.rs.Produces;

@RegisterRestClient
@Path("/external-api")
public interface ExternalService {

    @GET
    @Produces("application/json")
    String getData();
}
```
### **Inject & Use REST Client**
```java
import org.eclipse.microprofile.rest.client.inject.RestClient;
import jakarta.inject.Inject;
import jakarta.ws.rs.GET;
import jakarta.ws.rs.Path;

@Path("/fetch")
public class FetchResource {

    @Inject
    @RestClient
    ExternalService externalService;

    @GET
    public String fetchData() {
        return externalService.getData();
    }
}
```

---

## **8. Quarkus Reactive Messaging (Kafka Integration)**  
### **Add Kafka Dependency**  
```xml
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-smallrye-reactive-messaging-kafka</artifactId>
</dependency>
```
### **Producer (Send Message to Kafka)**
```java
import jakarta.enterprise.context.ApplicationScoped;
import org.eclipse.microprofile.reactive.messaging.Channel;
import org.eclipse.microprofile.reactive.messaging.Emitter;

@ApplicationScoped
public class KafkaProducer {
    @Channel("my-topic")
    Emitter<String> emitter;

    public void sendMessage(String message) {
        emitter.send(message);
    }
}
```
### **Consumer (Receive Messages from Kafka)**
```java
import jakarta.enterprise.context.ApplicationScoped;
import org.eclipse.microprofile.reactive.messaging.Incoming;

@ApplicationScoped
public class KafkaConsumer {

    @Incoming("my-topic")
    public void consume(String message) {
        System.out.println("Received: " + message);
    }
}
```
### **Kafka Configuration (`application.properties`)**  
```properties
mp.messaging.outgoing.my-topic.connector=smallrye-kafka
mp.messaging.outgoing.my-topic.topic=my-topic
mp.messaging.outgoing.my-topic.bootstrap.servers=localhost:9092

mp.messaging.incoming.my-topic.connector=smallrye-kafka
mp.messaging.incoming.my-topic.topic=my-topic
mp.messaging.incoming.my-topic.bootstrap.servers=localhost:9092
```

---

## **9. Security (JWT Authentication)**
### **Add Security Dependencies**
```xml
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-smallrye-jwt</artifactId>
</dependency>
```
### **Protect Endpoints with Roles**
```java
import jakarta.annotation.security.RolesAllowed;
import jakarta.ws.rs.GET;
import jakarta.ws.rs.Path;

@Path("/secure")
public class SecureResource {

    @GET
    @RolesAllowed("user")
    public String secureEndpoint() {
        return "This is a protected resource!";
    }
}
```
### **JWT Configuration (`application.properties`)**
```properties
mp.jwt.verify.publickey.location=META-INF/resources/publicKey.pem
quarkus.http.auth.permission.authenticated.paths=/secure
quarkus.http.auth.permission.authenticated.policy=authenticated
```

---

## **10. Testing with Quarkus**  
### **Add Dependency**
```xml
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-junit5</artifactId>
    <scope>test</scope>
</dependency>
```
### **Basic Test**
```java
import io.quarkus.test.junit.QuarkusTest;
import org.junit.jupiter.api.Test;
import static io.restassured.RestAssured.given;
import static org.hamcrest.Matchers.is;

@QuarkusTest
public class GreetingResourceTest {

    @Test
    public void testHelloEndpoint() {
        given()
          .when().get("/hello")
          .then()
             .statusCode(200)
             .body(is("Hello, Quarkus!"));
    }
}
```

