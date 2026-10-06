# ShopLite

A small online-shop order flow built as Java Spring Boot microservices: browse products, place an order, pay, and get notified. It exists to practise, end to end, what a senior Java microservices role expects.

**Covers:** Java 8–21 · Spring Boot 4 · Spring MVC · design patterns · SAGA (choreography and orchestration) · outbox · Kafka · OAuth2/JWT with Keycloak · Spring Cloud Gateway · multithreading · caching (Caffeine, Redis) · Resilience4j · observability · Docker · GitHub Actions · Kubernetes.

## Services

| Service | Role | Store |
| --- | --- | --- |
| catalog-service | Products and stock | Postgres |
| order-service | Orders, runs the SAGA | Postgres |
| payment-service | Charges by payment method | Postgres |
| notification-service | Sends order notifications | MongoDB |
| api-gateway | Single entry point, JWT, rate limiting | Redis |
| admin-web | Spring MVC + Thymeleaf admin UI | – |

## Build

Requires JDK 21 and Docker.

```bash
./mvnw verify
```

## Status

Built in phases; design decisions are recorded in [`docs/adr`](docs/adr).
