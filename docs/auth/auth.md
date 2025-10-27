# Auth

This is the interface for the Auth service. It defines the API for user authentication and authorization.

## File Structure

```mermaid
flowchart TD
    A[auth] --> B(pom.xml)
    A --> C(src)
    C --> D(main)
    D --> E(java)
    E --> F(store)
    F --> G(auth)
    G --> H(AuthController.java)
    G --> I(LoginIn.java)
    G --> J(RegisterIn.java)
    G --> K(TokenOut.java)
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
	<artifactId>auth</artifactId>
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

### AuthController.java

```java
package store.auth;

import java.util.Map;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;

@FeignClient(name = "auth", url = "http://auth:8080")
public interface AuthController {
    
    @PostMapping("/auth/register")
    public ResponseEntity<TokenOut> register(
        @RequestBody RegisterIn in
    );

    @PostMapping("/auth/login")
    public ResponseEntity<TokenOut> login(
        @RequestBody LoginIn in
    );

    @PostMapping("/auth/solve")
    public ResponseEntity<Map<String, String>> solve(
        @RequestBody TokenOut in
    );

}
```

### LoginIn.java

```java
package store.auth;

import lombok.Builder;

@Builder
public record LoginIn(
    String email,
    String password
) {
    
}
```

### RegisterIn.java

```java
package store.auth;

import lombok.Builder;

@Builder
public record RegisterIn(
    String name,
    String email,
    String password
) {
    
}
```

### TokenOut.java

```java
package store.auth;

import lombok.Builder;

@Builder
public record TokenOut (
    String jwt
) {
    
}
```
