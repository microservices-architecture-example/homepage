# Documentação dos Microserviços

## Estrutura atual dos microserviços

``` mermaid
flowchart LR
    subgraph api [Subnet API]
        direction TB
        gateway --> account
        gateway --> auth:::red
        gateway --> product
        gateway --> order
        gateway --> exchange
        auth --> account
        order --> product
        account --> db@{ shape: cyl, label: "Database" }
        product --> db
        order --> db
    end
    exchange e3@==> 3partyapi:::green@{label: "3rd-party API"}
    internet e2@==> |request| gateway:::orange
    e2@{ animate: true }
    e3@{ animate: true }
    classDef green fill:#cfc
    classDef orange fill:#FCBE3E
```

## Repositórios

Principal: 
[main](https://github.com/microservices-architecture-example/all)

| Microservice | Interface | Implementation |
|-|-|-|
| Account | [account](https://github.com/microservices-architecture-example/account) | [account-service](https://github.com/microservices-architecture-example/account.service) |
| Auth | [auth](https://github.com/microservices-architecture-example/auth) | [auth-service](https://github.com/microservices-architecture-example/auth.service) |
| Gateway |  | [gateway-service](https://github.com/microservices-architecture-example/gateway.service) |
| Product | [product](https://github.com/microservices-architecture-example/product) | [product-service](https://github.com/microservices-architecture-example/product.service) |
| Order | [order](https://github.com/microservices-architecture-example/order) | [order-service](https://github.com/microservices-architecture-example/order.service) |
| Exchange |  | [exchange-service](https://github.com/microservices-architecture-example/exchange.service) |