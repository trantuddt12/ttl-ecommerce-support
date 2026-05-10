# AGENTS.md

## Workspace scope

This workspace is a container for multiple projects, not a single buildable application from the root.

Top-level projects observed:
- `ecommerce-shop/`: Java 17, Spring Boot 3.3 multi-module backend.
- `ecommerce-ng/`: Angular 17 standalone frontend with SSR.
- `docs/`, `design.md`, `ROLE_PERMISSION.md`, `prompt.md`, and roadmap/progress files: supporting documentation.

If your task is implementation work, choose the correct project first and run commands from that project root, not from workspace root.

## First files to check

- `ecommerce-shop/AGENTS.md`
- `ecommerce-ng/AGENTS.md`
- `ecommerce-shop/README.md`
- `ecommerce-ng/README.md`
- `AI_PROGRESS.md`
- `AI_PROGRESS/*.md`

The two project-local `AGENTS.md` files already contain deeper project-specific guidance. This root file is for workspace routing and cross-project context.

## Repository routing guide

### Backend work
Use `ecommerce-shop/` when the task involves:
- Spring Boot APIs
- authentication/authorization
- product, brand, category, order, role, or user backend logic
- Kafka, Redis, Elasticsearch indexing/search sync
- Flyway/database changes
- scraper/import utilities

Authoritative local guide:
- `ecommerce-shop/AGENTS.md`

Additional AI guidance observed there:
- `ecommerce-shop/AI_START_HERE.md`
- `ecommerce-shop/.ai/BASELINE.md`
- `ecommerce-shop/.ai/instructions/*.md`
- `ecommerce-shop/.ai/references/*.md`
- `ecommerce-shop/.ai/skills/*/SKILL.md`
- `ecommerce-shop/.ai/agents/*.md`

### Frontend work
Use `ecommerce-ng/` when the task involves:
- Angular pages/components/routes
- auth/session UX
- admin/client UI flows
- API integration from the browser
- SSR/Express host behavior

Authoritative local guide:
- `ecommerce-ng/AGENTS.md`

Additional AI guidance observed there:
- `ecommerce-ng/AI/references/*.md`
- `ecommerce-ng/.agents/skills/angular-best-practices/AGENTS.md`

## Essential commands by project

## `ecommerce-shop/` backend
Run from `ecommerce-shop/`.

### Build and test
```bash
./mvnw clean install
./mvnw test
./mvnw -pl basecommerce test
./mvnw -pl coreservice test
./mvnw -pl searchservice test
```

### Run services
```bash
./mvnw -pl basecommerce spring-boot:run -Dspring-boot.run.arguments="--spring.profiles.active=local"
./mvnw -pl searchservice spring-boot:run
./mvnw.cmd spring-boot:run -Dspring-boot.run.arguments="--spring.profiles.active=mcp" -pl basecommerce
```

### Local infrastructure helpers documented in repo
```bash
docker compose -f basecommerce/src/main/resources/docker-kafka/docker-compose.yml up -d
docker compose -f basecommerce/src/main/resources/docker-kafka/es-kibana-docker.yml up -d
docker compose -f basecommerce/src/main/resources/docker-mailpit/docker-compose.yml up -d
```

### Scraper workflow
```bash
./mvnw clean compile
./mvnw package -DskipTests -pl scraper
java -jar scraper/target/scraper-0.0.1-SNAPSHOT-jar-with-dependencies.jar
```

## `ecommerce-ng/` frontend
Run from `ecommerce-ng/`.

```bash
npm install
npm run start
npm run build
npm run watch
npm run test -- --watch=false --browsers=ChromeHeadless
npm run serve:ssr:ecommerce-ng
```

Observed in `package.json`:
- no lint script
- no formatter script

## Architecture map across the workspace

### Backend/frontend split
- `ecommerce-shop/` is the API/data/search side.
- `ecommerce-ng/` is the web client/admin UI.

### Backend high-level composition
Observed in `ecommerce-shop/pom.xml` and app entrypoints:
- `basecommerce`: main runtime app, `@SpringBootApplication(scanBasePackages = "com.ttl")`
- `coreservice`: shared auth/security/response wrapper layer
- `common`: shared DTOs/events/constants
- `searchservice`: separate Spring Boot search/indexing process with datasource/JPA autoconfig excluded
- `scraper`: standalone scraping/import utility

Important backend runtime flow already confirmed by source:
- catalog writes in `basecommerce` use cache invalidation/versioning and publish Kafka events after commit
- `searchservice` consumes catalog topics and reindexes products
- `ResponseHandler` wraps most JSON responses into `{ data, ... }`, so declared controller return types are not always wire-format shapes

### Frontend high-level composition
Observed in `ecommerce-ng/src/app`:
- `core/`: auth, HTTP, interceptors, guards, layout, state, constants, utilities
- `features/`: domain pages grouped by business area
- `shared/ui/`: app-wide reusable UI widgets

Important frontend runtime flow already confirmed by source:
- bootstrap goes through `src/app/app.config.ts`
- all HTTP is expected to go through `BaseApiService`
- interceptor order is meaningful: loading -> credentials -> auth token -> refresh token -> error mapping
- auth restore starts in `AppInitService`
- access token is stored in `sessionStorage`; refresh is expected via cookie-backed endpoint

## Cross-project gotchas

- This workspace has multiple `AGENTS.md` files. Prefer the closest one to the code you are changing.
- Do not assume root-level commands exist for build/test/run; none were observed for the whole workspace.
- Documentation-only changes (`*.md`, `docs/**`, AI guidance/progress logs, design/roadmap files) do not require code build/test commands; verify by reading back the edited document instead.
- Backend responses may be envelope-wrapped while frontend code sometimes needs explicit unwrapping; check both `ecommerce-shop/coreservice/src/main/java/com/ttl/core/handler/ResponseHandler.java` and `ecommerce-ng/src/app/core/models/auth.models.ts` before changing API contracts.
- `AI_PROGRESS.md` va `AI_PROGRESS/*.md` o workspace root la nhat ky tien trinh chuan. Them muc moi vao file ngay hien tai trong `AI_PROGRESS/`, va giu `AI_PROGRESS.md` lam muc luc/huong dan.
- The frontend and backend each have their own AI/reference material; do not mix frontend conventions into backend modules or vice versa.

## Observed conventions worth knowing

### Backend
- Controllers are expected to stay thin; business logic belongs in services.
- DTO/shared contracts belong in `common` when there is real cross-module reuse.
- Cache + event publishing is a standard pattern in catalog write flows.
- JUnit 5 + Mockito are the observed backend test style.

### Frontend
- Angular standalone components throughout.
- Domain-first feature folders.
- Signals-based auth state in `AuthStore`.
- Many pages use inline templates/styles; small edits are safer than broad rewrites.
- `.editorconfig` enforces UTF-8, spaces, 2-space indent, final newline, and single quotes in TypeScript.
- Karma/Jasmine is the configured unit test setup; only minimal test coverage is currently present.

## Practical workflow for future agents

1. Identify whether the task belongs to backend, frontend, or both.
2. Open the nearest project-local `AGENTS.md` before making changes.
3. Run commands from that project root.
4. If touching API integration, verify response-envelope behavior on both sides.
5. If touching backend architecture or conventions, also read `ecommerce-shop/.ai/BASELINE.md` and related scoped docs.
6. If touching frontend API/auth flows, also read `ecommerce-ng/AI/references/http-and-api.md`, `auth-flow.md`, and `conventions.md`.
7. For progress logging, do not use legacy project-local `AI_PROGRESS.md` files; append to the current day file under root `AI_PROGRESS/` and keep `AI_PROGRESS.md` updated as the index.
7. For progress logging, do not use legacy project-local `AI_PROGRESS.md` files; append to the current day file under root `AI_PROGRESS/` and keep `AI_PROGRESS.md` updated as the index.

## Files that save time early

### Backend
- `ecommerce-shop/AGENTS.md`
- `ecommerce-shop/AI_START_HERE.md`
- `ecommerce-shop/.ai/BASELINE.md`
- `ecommerce-shop/pom.xml`
- `ecommerce-shop/basecommerce/src/main/java/com/ttl/base/BaseCommerceApplication.java`
- `ecommerce-shop/searchservice/src/main/java/com/search/SearchServiceApplication.java`
- `ecommerce-shop/coreservice/src/main/java/com/ttl/core/handler/ResponseHandler.java`

### Frontend
- `ecommerce-ng/AGENTS.md`
- `ecommerce-ng/package.json`
- `ecommerce-ng/src/app/app.config.ts`
- `ecommerce-ng/src/app/app.routes.ts`
- `ecommerce-ng/src/app/core/http/base-api.service.ts`
- `ecommerce-ng/src/app/core/services/auth.service.ts`
- `ecommerce-ng/src/app/core/services/app-init.service.ts`
- `ecommerce-ng/src/app/core/models/auth.models.ts`
