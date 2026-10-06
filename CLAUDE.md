# ShopLite

Spring Boot microservices order flow (catalog → order → payment → notification) built to prepare for senior Java interviews. The owner builds it to learn: **explain the why behind every change**, not just the what.

- Spec + build checklist (single source of truth): Notion page `3f1251d7d623802e8fd8d726bc974d76`
- Interview notes: Notion "Master Interview Reviewer" `3ea251d7d62381a987cbc0ec5161ab77`
- Decisions: `docs/adr/`

## Stack

Java 21 · Spring Boot 4.0.x · Spring Cloud 2025.1.x · Maven multi-module (use `./mvnw`) · Postgres · MongoDB · Kafka · Redis · Keycloak · Docker · kind + Helm. AWS only on Free plan credits, never EKS/NAT Gateway.

## Layout

```
pom.xml                 parent: Boot parent + Spring Cloud BOM, no versions in child POMs
<service>/              one Maven module per service (created with /new-service)
java-lab/, security-lab/  learning modules, never deployed
docs/adr/               architecture decision records
.claude/                shared agents and skills
```

Base package `com.shoplite.<service>`. Ports: gateway 8080, catalog 8081, order 8082, payment 8083, notification 8084, admin-web 8090, Keycloak 8180.

## Commands

- Build + test everything: `./mvnw verify` (Windows: `.\mvnw.cmd verify`)
- One module: `./mvnw -pl catalog-service -am verify`
- Run a service: `./mvnw -pl catalog-service spring-boot:run`

## Java and Spring rules

- Constructor injection only; dependencies are `final`. No field injection.
- Singleton beans are stateless.
- DTOs and events are `record`s. Never expose JPA entities from a controller.
- `Optional` only as a return type, never a field or parameter.
- JPA associations stay LAZY; fix N+1 with fetch joins or entity graphs, never EAGER.
- Remember proxies: `@Transactional`, `@Cacheable`, `@Async` do nothing on self-invocation.
- One error shape for every API, from a `@RestControllerAdvice`.
- Every remote call has a timeout. Retries are bounded with backoff.
- Kafka events are keyed by `orderId`. Consumers are idempotent.
- Config in `application.yml` + profiles; secrets only from env vars or Kubernetes Secrets. Never commit a secret.

## Tests

- New behaviour ships with a test. Prefer slices: `@WebMvcTest`, `@DataJpaTest`; `@SpringBootTest` only for wiring/integration.
- Real infrastructure in tests through Testcontainers, not H2.
- A bug or race condition is reproduced by a failing test before it is fixed.

## Workflow

- Work phase by phase from the Notion checklist. `/status` says where we are; `/checkpoint` records progress.
- End of each phase: tests green → ADR via the `architect` agent → `/to-answer` → `/quiz` → `/checkpoint`.
- The owner asks questions mid-build. Answer plainly, point to the code in this repo, give the short interview answer, then continue.
- Commit in small steps with clear messages. Ask before pushing, tagging, or writing to Notion.

## Git and PRs

- Never commit to `main` directly. Every change goes through a pull request.
- Branch per task from an up-to-date `main`: `phase-N/short-name` (e.g. `phase-1/catalog-crud`).
- Conventional commit messages: `feat:`, `fix:`, `test:`, `docs:`, `chore:`, `refactor:`.
- Push the branch and open a PR with `gh pr create`, filling `.github/pull_request_template.md`. The "Interview angle" section is required.
- Squash-merge into `main`, delete the branch, then `git switch main && git pull`.
- Phase tags (`phase-N-done`) go on `main` after the phase's last PR is merged.
