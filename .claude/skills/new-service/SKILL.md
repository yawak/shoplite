---
name: new-service
description: Scaffold a new ShopLite Spring Boot service module with the standard layout, Actuator, error handling and a first test, and register it in the parent POM. Use when a phase needs a new service or module.
disable-model-invocation: true
argument-hint: "<module-name, e.g. catalog-service> [port]"
---

# /new-service $ARGUMENTS

Create the module `$0` (port `$1`, or the one listed in `CLAUDE.md`). Explain each file briefly as you create it; the owner is learning from this.

## Files

```
$0/
  pom.xml
  src/main/java/com/shoplite/<name>/<Name>Application.java
  src/main/java/com/shoplite/<name>/api/ApiExceptionHandler.java
  src/main/resources/application.yml
  src/test/java/com/shoplite/<name>/<Name>ApplicationTests.java
```

`<name>` = module name without `-service` (e.g. `catalog`); `<Name>` = capitalised.

- **pom.xml:** parent `com.shoplite:shoplite-parent:0.1.0-SNAPSHOT` (relativePath `../pom.xml`), no dependency versions. Start with: `spring-boot-starter-webmvc`, `spring-boot-starter-validation`, `spring-boot-starter-actuator`, `spring-boot-starter-test` (test). Add data/messaging starters only when the phase needs them. Include `spring-boot-maven-plugin`.
- **Application class:** plain `@SpringBootApplication`.
- **ApiExceptionHandler:** `@RestControllerAdvice` returning `ProblemDetail` for validation errors (400), not-found (404) and unexpected errors (500, no stack trace in the body). One error shape for the whole API.
- **application.yml:** `spring.application.name`, `server.port`, Actuator exposing `health,info,metrics` with liveness/readiness probes enabled, graceful shutdown.
- **Test:** context-loads test plus one `@WebMvcTest`-style check that `/actuator/health` returns UP (use `@SpringBootTest(webEnvironment = RANDOM_PORT)` for the actuator check).

## Then

1. Add `<module>$0</module>` to the root `pom.xml`.
2. Run `./mvnw -pl $0 -am verify` and fix anything failing.
3. Report the files created and the test result. Do not commit; the owner reviews first.
