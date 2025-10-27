# Account

This is the interface for the Account service. It defines the API for managing user accounts.

## File Structure

```mermaid
flowchart TD
    A[account] --> B(pom.xml)
    A --> C(src)
    C --> D(main)
    D --> E(java)
    E --> F(store)
    F --> G(account)
    G --> H(AccountController.java)
    G --> I(AccountIn.java)
    G --> J(AccountOut.java)
    G --> K(Role.java)
```

## Source Code

### pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.5.5</version>
        <relativePath/>
    </parent>

    <groupId>store</groupId>
    <artifactId>account</artifactId>
    <version>1.0.0</version>

    <properties>
        <java.version>21</java.version>
        <spring-cloud.version>2025.0.0</spring-cloud.version>
        <maven.compiler.proc>full</maven.compiler.proc>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
    </dependencies>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

</project>
```

### AccountController.java

```java
package store.account;

import java.util.List;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestHeader;

@FeignClient(name = "account", url = "http://account:8080")
public interface AccountController {
    
    @PostMapping("/account")
    public ResponseEntity<AccountOut> create(
        @RequestBody AccountIn in
    );

    @GetMapping("/account/{id}")
    public ResponseEntity<AccountOut> findById(
        @PathVariable("id") String id
    );

    @PostMapping("/account/login")
    public ResponseEntity<AccountOut> findByEmailAndPassword(
        @RequestBody AccountIn in
    );

    @GetMapping("/account")
    public ResponseEntity<List<AccountOut>> findAll(
    );

    @DeleteMapping("/account/{id}")
    public ResponseEntity<Void> delete(
        @PathVariable("id") String id
    );

    @GetMapping("/account/whoami")
    public ResponseEntity<AccountOut> whoAmI(
        @RequestHeader(value = "id-account", required = true) String idAccount
    );

}
```

### AccountIn.java

```java
package store.account;

import lombok.Builder;

@Builder
public record AccountIn(
    String name,
    String email,
    String password
) {
    
}
```

### AccountOut.java

```java
package store.account;

import lombok.Builder;

@Builder
public record AccountOut(
    String id,
    String name,
    String email,
    Role role
) {
    
}
```

### Role.java

```java
package store.account;

public enum Role {
    USER,
    ADMIN
}
```
