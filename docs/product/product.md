# Product API

A **Product API** implementa o CRUD de produtos do domínio `store`, seguindo o mesmo padrão adotado no projeto: **interface** (`product`) e **service** (`product.service`) por trás do **gateway** e protegido por **JWT**.

!!! info "Trusted layer e segurança"
    Toda requisição externa entra pelo **gateway**.  
    As rotas `/product/**` são **protegidas**: é obrigatório enviar `Authorization: Bearer <jwt>`.

---

## Visão geral

- **Interface (`product`)**: define o contrato (DTOs e Feign) consumido por outros módulos/fronts.
- **Service (`product.service`)**: implementação REST, regras de negócio, persistência (JPA), e migrações (Flyway).

``` mermaid
classDiagram
    namespace product {
        class ProductController {
            +create(ProductIn ProductIn): ProductOut
            +delete(String id): void
            +findAll(): List<ProductOut>
            +findById(String id): ProductOut
        }
        class ProductIn {
            -String name
            -Double price
            -String unit
        }
        class ProductOut {
            -String id
            -String name
            -Double price
            -String unit
        }
    }
    namespace product.service {
        class ProductResource {
            +create(ProductIn ProductIn): ProductOut
            +delete(String id): void
            +findAll(): List<ProductOut>
            +findById(String id): ProductOut
        }
        class ProductService {
            +create(ProductIn ProductIn): ProductOut
            +delete(String id): void
            +findAll(): List<ProductOut>
            +findById(String id): ProductOut
        }
        class ProductRepository {
            +create(ProductIn ProductIn): ProductOut
            +delete(String id): void
            +findAll(): List<ProductOut>
            +findById(String id): ProductOut
        }
        class Product {
            -String id
            -String name
            -Double price
            -String unit
        }
        class ProductModel {
            +create(ProductIn ProductIn): ProductOut
            +delete(String id): void
            +findAll(): List<ProductOut>
            +findById(String id): ProductOut
        }
    }
    <<Interface>> ProductController
    ProductController ..> ProductIn
    ProductController ..> ProductOut

    <<Interface>> ProductRepository
    ProductController <|-- ProductResource
    ProductResource *-- ProductService
    ProductService *-- ProductRepository
    ProductService ..> Product
    ProductService ..> ProductModel
    ProductRepository ..> ProductModel
```

## Estrutura da requisição

``` mermaid
flowchart LR
    subgraph api [Trusted Layer]
        direction TB
        gateway --> account
        gateway --> auth
        account --> db@{ shape: cyl, label: "Database" }
        auth --> account
        gateway e5@==>|""| product:::red
        product e2@==>|""| db
    end
    internet e1@==>|request| gateway
    e1@{ animate: true }
    e2@{ animate: true }
    e5@{ animate: true }
    classDef red fill:#fcc
    click product "#product-api" "Product API"
```

## Product


``` tree
api/
    product/
        src/
            main/
                java/
                    store/
                        product/
                            ProductController.java
                            ProductIn.java
                            ProductOut.java
        pom.xml
        Jenkinsfile
```

??? info "Source"

    === "pom.xml"

        ``` { .yaml .copy .select linenums="1" }
        --8<-- "https://raw.githubusercontent.com/microservices-architecture-example/product/refs/heads/main/pom.xml"
        ```

    === "Jenkinsfile"

        ``` { .jenkinsfile .copy .select linenums="1" }
        --8<-- "https://raw.githubusercontent.com/microservices-architecture-example/product/refs/heads/main/Jenkinsfile"
        ```

    === "ProductController"

        ``` { .java title='ProductController.java' .copy .select linenums='1' }
        --8<-- "https://raw.githubusercontent.com/microservices-architecture-example/product/refs/heads/main/src/main/java/store/product/ProductController.java"
        ```

    === "ProductIn"

        ``` { .java title='ProductIn.java' .copy .select linenums='1' }
        --8<-- "https://raw.githubusercontent.com/microservices-architecture-example/product/refs/heads/main/src/main/java/store/product/ProductIn.java"
        ```

    === "ProductOut"

        ``` { .java title='ProductOut.java' .copy .select linenums='1' }
        --8<-- "https://raw.githubusercontent.com/microservices-architecture-example/product/refs/heads/main/src/main/java/store/product/ProductOut.java"
        ```

<!-- termynal -->

``` { bash }
> mvn clean install
```

## product.service

``` tree
api/
    product.service/
        k8s/
            k8s.yaml
        src/
            main/
                java/
                    store/
                        product/
                            Product.java
                            ProductApplication.java
                            ProductModel.java
                            ProductParser.java
                            ProductRepository.java
                            ProductResource.java
                            ProductService.java
                            RedisCacheConfig.java
                resources/
                    application.yaml
                    db/
                        migration/
                            V2025.08.29.001__create_schema.sql
                            V2025.08.29.002__create_table_product.sql
        pom.xml
        Dockerfile
        Jenkinsfile
```

??? info "Source"

    === "pom.xml"

        ``` { .yaml .copy .select linenums="1" }
        --8<-- "https://raw.githubusercontent.com/microservices-architecture-example/product.service/refs/heads/main/pom.xml"
        ```

    === "Dockerfile"

        ``` { .dockerfile .copy .select linenums="1" }
        --8<-- "https://raw.githubusercontent.com/microservices-architecture-example/product.service/refs/heads/main/Dockerfile"
        ```

    === "Jenkinsfile"

        ``` { .jenkinsfile .copy .select linenums="1" }
        --8<-- "https://raw.githubusercontent.com/microservices-architecture-example/product.service/refs/heads/main/Jenkinsfile"
        ```

    === "k8s.yaml"

        ``` { .yaml .copy .select linenums="1" }
        --8<-- "https://raw.githubusercontent.com/microservices-architecture-example/product.service/refs/heads/main/k8s/k8s.yaml"
        ```

    === "application.yaml"

        ``` { .yaml .copy .select linenums="1" }
        --8<-- "https://raw.githubusercontent.com/microservices-architecture-example/product.service/refs/heads/main/src/main/resources/application.yaml"
        ```

    === "Product.java"

        ``` { .java .copy .select linenums='1' }
        --8<-- "https://raw.githubusercontent.com/microservices-architecture-example/product.service/refs/heads/main/src/main/java/store/product/Product.java"
        ```

    === "ProductApplication.java"

        ``` { .java .copy .select linenums='1' }
        --8<-- "https://raw.githubusercontent.com/microservices-architecture-example/product.service/refs/heads/main/src/main/java/store/product/ProductApplication.java"
        ```

    === "ProductModel.java"

        ``` { .java .copy .select linenums='1' }
        --8<-- "https://raw.githubusercontent.com/microservices-architecture-example/product.service/refs/heads/main/src/main/java/store/product/ProductModel.java"
        ```

    === "ProductParser.java"

        ``` { .java .copy .select linenums='1' }
        --8<-- "https://raw.githubusercontent.com/microservices-architecture-example/product.service/refs/heads/main/src/main/java/store/product/ProductParser.java"
        ```

    === "ProductRepository.java"

        ``` { .java .copy .select linenums='1' }
        --8<-- "https://raw.githubusercontent.com/microservices-architecture-example/product.service/refs/heads/main/src/main/java/store/product/ProductRepository.java"
        ```

    === "ProductResource.java"

        ``` { .java .copy .select linenums='1' }
        --8<-- "https://raw.githubusercontent.com/microservices-architecture-example/product.service/refs/heads/main/src/main/java/store/product/ProductResource.java"
        ```

    === "ProductService.java"

        ``` { .java .copy .select linenums='1' }
        --8<-- "https://raw.githubusercontent.com/microservices-architecture-example/product.service/refs/heads/main/src/main/java/store/product/ProductService.java"
        ```

    === "RedisCacheConfig.java"

        ``` { .java .copy .select linenums='1' }
        --8<-- "https://raw.githubusercontent.com/microservices-architecture-example/product.service/refs/heads/main/src/main/java/store/product/RedisCacheConfig.java"
        ```

    === "V2025.08.29.001__create_schema.sql"

        ``` { .sql .copy .select linenums="1" }
        --8<-- "https://raw.githubusercontent.com/microservices-architecture-example/product.service/refs/heads/main/src/main/resources/db/migration/V2025.08.29.001__create_schema.sql"
        ```

    === "V2025.08.29.002__create_table_product.sql"

        ``` { .sql .copy .select linenums="1" }
        --8<-- "https://raw.githubusercontent.com/microservices-architecture-example/product.service/refs/heads/main/src/main/resources/db/migration/V2025.08.29.002__create_table_product.sql"
        ```

<!-- termynal -->

``` { bash }
> mvn clean package spring-boot:run
```