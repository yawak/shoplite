# 0001. Monorepo, Maven multi-module, Java 21, Spring Boot 4.0

- **Status:** Accepted
- **Date:** 2026-10-06
- **Phase:** 0

## Context

ShopLite has four services, a gateway, a web UI and two learning modules, built by one person. Services must stay independently deployable, but shared versions and one build make a solo project manageable. The Java and Spring versions should match what current job postings ask for.

## Decision

One Git repository with a Maven multi-module build. The parent POM inherits `spring-boot-starter-parent` 4.0.8 and imports the Spring Cloud 2025.1.3 BOM, so child modules declare no versions. Java 21 (LTS), enforced by the Maven Enforcer plugin. The Maven wrapper pins Maven 3.9.11.

## Alternatives considered

- **Repo per service:** the usual choice for large teams with separate owners. For one developer it multiplies CI setup and version drift with no benefit.
- **Gradle:** equally capable; Maven is more common in Java job postings and is already installed.
- **Spring Boot 3.5:** its open-source support ended in June 2026 (last release 3.5.16). New work should start on 4.0.
- **Spring Boot 4.1:** newest, but Spring Cloud has no GA release train for it yet; 4.0.x with Spring Cloud 2025.1 is the supported pair.
- **Java 25:** a newer LTS, but Java 21 is what most employers run today and covers virtual threads, records, sealed types and pattern matching.

## Consequences

- Easier: one `./mvnw verify` builds and tests everything; versions live in one place; refactors across services are one commit.
- Harder: services can accidentally share code through the parent. Rule: no shared business library; each service owns its model.
- Watch: each service still gets its own Docker image and Helm release, so deployability stays independent.

## Interview angle

- **Short answer:** "Monorepo for a small team, with independent deployables per module; the parent POM only centralises versions, not business code."
- **Likely follow-up:** "Doesn't a monorepo couple your microservices?" Coupling comes from shared code and shared databases, not from sharing a repo. Each service has its own module, database, image and release, and there is no shared domain library.
