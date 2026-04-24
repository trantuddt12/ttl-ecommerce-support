# Spring Boot 2-Month Daily Roadmap

Muc tieu cua roadmap nay:

- Di tu muc mid-level len senior-ready ky thuat trong 8 tuan
- Hoc 8 tieng/ngay, 6 ngay/tuan
- Bam sat project hien tai: `ecommerce-shop`
- Moi ngay phai tao ra it nhat 1 artifact huu hinh

Pham vi kien thuc phai nam duoc sau 2 thang:

- Spring Boot core
- Spring Security
- JPA va transaction
- Business logic design
- Testing strategy
- Production mindset
- Refactor va design note theo tu duy senior

## Cach hoc moi ngay

Khung gio goi y:

1. `08:00 - 10:00`: hoc ly thuyet dung chu de ngay hom do
2. `10:15 - 12:15`: doc code va trace flow trong project
3. `13:30 - 15:30`: lam bai tap phan tich, test plan, refactor plan, hoac code nho
4. `15:45 - 16:45`: chay test, doc log, debug, tong hop van de
5. `16:45 - 17:30`: viet note tong ket trong ngay

Quy tac bat buoc:

- Moi ngay phai co it nhat 1 artifact
- Moi artifact phai luu trong `docs/` hoac so tay hoc tap rieng
- Khong hoc lan man ngoai chu de cua ngay
- Neu doc code ma khong viet note thi xem nhu chua hoc xong

## Cach danh dau hoan thanh

Moi ngay danh dau theo checklist sau:

- [ ] Da hoc ly thuyet dung chu de
- [ ] Da doc code dung file can tap trung
- [ ] Da tao artifact
- [ ] Da tong ket duoc 3 dieu moi hoc
- [ ] Da viet ra it nhat 1 risk hoac 1 tradeoff ky thuat

---

## Tuan 1: Biet ro project dang chay nhu the nao

Muc tieu tuan:

- Hieu kien truc tong the cua `ecommerce-shop`
- Biet vai tro tung module
- Trace duoc request flow co ban
- On lai Spring Boot core theo code that

### Ngay 1: Workspace map

- [ ] Doc `ecommerce-shop/pom.xml`
- [ ] Liet ke tat ca module va vai tro cua tung module
- [ ] Xac dinh module nao la core business, module nao la shared, module nao la phu tro
- [ ] Ve so do module o muc high-level

File can doc:

- `pom.xml`
- `basecommerce/pom.xml` neu co
- `coreservice/pom.xml` neu co

Artifact bat buoc:

- `docs/day-01-module-map.md`

Cau hoi phai tra loi:

- Vi sao project tach thanh nhieu module?
- `common` dang dung de chua cai gi?
- Module nao co nguy co coupling cao?

### Ngay 2: Startup flow va bean scanning

- [ ] Doc file application entrypoint
- [ ] Xac dinh `scanBasePackages` dang quet den dau
- [ ] Ghi lai bean duoc tao theo kieu nao: `@Service`, `@Repository`, `@Configuration`, `@Component`
- [ ] Ghi note startup flow cua Spring Boot trong project nay

File can doc:

- `basecommerce/src/main/java/com/ttl/base/BaseCommerceApplication.java`
- `searchservice/src/main/java/com/search/SearchServiceApplication.java`

Artifact bat buoc:

- `docs/day-02-startup-flow.md`

Cau hoi phai tra loi:

- `@SpringBootApplication(scanBasePackages = "com.ttl")` tac dong gi?
- `@EnableJpaAuditing` duoc dung de lam gi?

### Ngay 3: Request lifecycle trong Spring MVC

- [ ] Chon 1 endpoint don gian de trace tu controller den response
- [ ] Ve lai flow: HTTP request -> controller -> service -> repository -> DB -> response
- [ ] Ghi lai request object va response object duoc map ra sao

File can doc:

- `basecommerce/src/main/java/com/ttl/base/controller/BrandController.java`
- `basecommerce/src/main/java/com/ttl/base/service/BrandService.java`
- `basecommerce/src/main/java/com/ttl/base/repositories/BrandRepository.java`

Artifact bat buoc:

- `docs/day-03-request-lifecycle.md`

Cau hoi phai tra loi:

- Controller dang lam viec gi?
- Service dang lam viec gi?
- Repository dang dong vai tro gi?

### Ngay 4: DTO, validation, response contract

- [ ] Doc 2-3 request DTO va 2-3 response DTO
- [ ] Xac dinh validation nao dang nam o DTO
- [ ] Liet ke validation nao dang nam o service
- [ ] Danh gia response contract co dong nhat hay khong

File can doc:

- `common/src/main/java/com/ttl/common/request/*.java`
- `common/src/main/java/com/ttl/common/response/*.java`
- `coreservice/src/main/java/com/ttl/core/controler/AuthController.java`

Artifact bat buoc:

- `docs/day-04-validation-and-response.md`

Cau hoi phai tra loi:

- Validation nao nen o DTO?
- Validation nao bat buoc phai o service?
- API hien tai co thong nhat cach tra loi khi loi khong?

### Ngay 5: Exception va error handling

- [ ] Tim toan bo custom exception trong project
- [ ] Ghi lai exception nao la business, exception nao la technical
- [ ] Xac dinh project co global exception handling hay chua
- [ ] Danh gia diem yeu trong cach tra loi loi hien tai

File can doc:

- `common/src/main/java/com/ttl/common/exception/*.java`
- controller co xu ly `try/catch`

Artifact bat buoc:

- `docs/day-05-exception-strategy.md`

Cau hoi phai tra loi:

- `BussinessException` dang duoc dung dung muc dich chua?
- Vi sao senior thuong uu tien global exception mapping?

### Ngay 6: Tong hop tuan 1

- [ ] Doc lai tat ca note tu ngay 1 den ngay 5
- [ ] Gom lai thanh 1 tai lieu tong hop tuan
- [ ] Viet 10 nhan xet ky thuat ve codebase hien tai
- [ ] Liet ke 5 diem manh va 5 diem chua senior-grade

Artifact bat buoc:

- `docs/week-01-summary.md`

---

## Tuan 2: Auth flow va Spring Security

Muc tieu tuan:

- Hieu sau luong auth hien tai
- Nam duoc Spring Security filter chain
- Hieu JWT, refresh token, revoke state, cookie policy

### Ngay 7: Security config overview

- [ ] Doc `SecurityConfig.java`
- [ ] Liet ke endpoint public va endpoint private
- [ ] Ghi lai luong filter chain co ban
- [ ] Ghi lai authorization dang duoc kiem soat theo cach nao

File can doc:

- `coreservice/src/main/java/com/ttl/core/config/SecurityConfig.java`

Artifact bat buoc:

- `docs/day-07-security-config-overview.md`

Cau hoi phai tra loi:

- `SecurityFilterChain` thay the gi trong Spring moi?
- `@EnableMethodSecurity` dong vai tro gi?

### Ngay 8: JwtAuthFilter

- [ ] Doc ky `JwtAuthFilter.java`
- [ ] Trace luong token tu request header vao `SecurityContextHolder`
- [ ] Ghi lai authentication object duoc tao ra nhu the nao
- [ ] Danh gia cach xu ly auth failure

File can doc:

- `coreservice/src/main/java/com/ttl/core/config/JwtAuthFilter.java`

Artifact bat buoc:

- `docs/day-08-jwt-auth-filter.md`

Cau hoi phai tra loi:

- Token duoc validate o dau?
- Revoke state duoc check o dau?
- Vi sao filter phai stop chain sau khi auth failure?

### Ngay 9: Auth service va token lifecycle

- [ ] Doc `AuthService.java`
- [ ] Ghi lai login, refresh, logout, get current user dang chay the nao
- [ ] Liet ke dependency cua `AuthService`
- [ ] Danh gia phan nao dang la orchestration, phan nao dang la business logic

File can doc:

- `coreservice/src/main/java/com/ttl/core/service/AuthService.java`
- `coreservice/src/main/java/com/ttl/core/service/LoginService.java`
- `coreservice/src/main/java/com/ttl/core/service/RefreshTokenService.java`

Artifact bat buoc:

- `docs/day-09-auth-service-lifecycle.md`

Cau hoi phai tra loi:

- Access token va refresh token duoc quan ly khac nhau nhu the nao?
- Logout hien tai co revoke du nhung thu can thiet chua?

### Ngay 10: Auth controller va API design

- [ ] Doc `AuthController.java`
- [ ] Liet ke tat ca endpoint auth
- [ ] Danh gia controller co chua qua nhieu logic hay khong
- [ ] Danh dau logic nao nen day xuong service hoac utility

File can doc:

- `coreservice/src/main/java/com/ttl/core/controler/AuthController.java`

Artifact bat buoc:

- `docs/day-10-auth-controller-review.md`

Cau hoi phai tra loi:

- `buildRefreshCookie` co nen nam o controller khong?
- `try/catch` o login endpoint da hop ly chua?

### Ngay 11: Cookie, CORS, refresh token rotation

- [ ] On ly thuyet `HttpOnly`, `Secure`, `SameSite`
- [ ] Phan tich config CORS hien tai
- [ ] Ghi lai refresh token rotation mang lai loi ich gi
- [ ] Liet ke 5 risk auth co the xay ra trong production

File can doc:

- `coreservice/src/main/java/com/ttl/core/config/SecurityConfig.java`
- cac file lien quan refresh token

Artifact bat buoc:

- `docs/day-11-cookie-cors-refresh.md`

Cau hoi phai tra loi:

- Khi nao `SameSite=Strict` gay van de?
- Tai sao auth qua cookie va auth qua bearer header co tradeoff khac nhau?

### Ngay 12: Tong hop auth flow

- [ ] Ve 1 so do auth flow day du
- [ ] Viet checklists review security cho project nay
- [ ] Viet 10 findings theo goc nhin senior
- [ ] Tong hop tuan 2

Artifact bat buoc:

- `docs/week-02-auth-flow-summary.md`
- `docs/week-02-security-checklist.md`

---

## Tuan 3: JPA, entity, repository, truy van

Muc tieu tuan:

- Hieu model du lieu cua project
- Nam fetch strategy, `@EntityGraph`, pagination, specification
- Nhin ra N+1 va query risk

### Ngay 13: Domain relationship map

- [ ] Doc entity lien quan den order, inventory, product, variant, cart
- [ ] Ve so do quan he entity
- [ ] Danh dau quan he nao de gay N+1

File can doc:

- entity trong `basecommerce/src/main/java/com/ttl/base/entities/`

Artifact bat buoc:

- `docs/day-13-domain-relationship-map.md`

### Ngay 14: Repository strategy

- [ ] Doc `OrderRepository.java`
- [ ] Doc 2-3 repository quan trong khac
- [ ] Liet ke query method chinh
- [ ] Ghi lai vi sao `@EntityGraph` duoc dat o do

File can doc:

- `basecommerce/src/main/java/com/ttl/base/repositories/OrderRepository.java`
- `ProductRepository.java`
- `InventoryRepository.java`

Artifact bat buoc:

- `docs/day-14-repository-strategy.md`

### Ngay 15: Lazy loading, eager loading, EntityGraph

- [ ] On ly thuyet lazy vs eager
- [ ] Doi chieu ly thuyet voi entity va repository hien tai
- [ ] Viet note nhung noi nao co nguy co query thua

Artifact bat buoc:

- `docs/day-15-fetch-strategy.md`

### Ngay 16: Pagination, specification, filter

- [ ] Trace endpoint list order
- [ ] Ghi lai filter nao dang duoc ho tro
- [ ] Danh gia sorting field co duoc whitelist an toan chua
- [ ] Xac dinh field nen co index

File can doc:

- `basecommerce/src/main/java/com/ttl/base/controller/OrderController.java`
- `basecommerce/src/main/java/com/ttl/base/service/OrderService.java`
- `OrderSpecification` neu co

Artifact bat buoc:

- `docs/day-16-pagination-filter-index.md`

### Ngay 17: Validation o entity va DB-level thinking

- [ ] Doc `EntityValidationTest.java`
- [ ] Doi chieu validation entity voi validation o DTO/service
- [ ] Ghi lai cai gi nen enforce bang code, cai gi nen enforce bang DB

File can doc:

- `basecommerce/src/test/java/com/ttl/base/entities/EntityValidationTest.java`

Artifact bat buoc:

- `docs/day-17-entity-validation.md`

### Ngay 18: Tong hop JPA tuan 3

- [ ] Tong hop cac risk JPA chinh
- [ ] Tong hop index proposal
- [ ] Viet 10 quy tac lam viec voi JPA cho rieng project nay

Artifact bat buoc:

- `docs/week-03-jpa-summary.md`

---

## Tuan 4: Order domain, transaction, state machine, concurrency

Muc tieu tuan:

- Hieu sau `OrderService`
- Nam transaction boundary
- Ve duoc state machine va risk concurrency

### Ngay 19: Cat nho `OrderService`

- [ ] Doc phan create va createFromCart trong `OrderService`
- [ ] Tach nhom responsibility bang note
- [ ] Liet ke private helper dang phuc vu nhom nao

File can doc:

- `basecommerce/src/main/java/com/ttl/base/service/OrderService.java`

Artifact bat buoc:

- `docs/day-19-order-service-responsibilities.md`

### Ngay 20: Order state machine

- [ ] Liet ke tat ca order status
- [ ] Ve state machine tuong ung
- [ ] Ghi lai transition nao hop le, transition nao khong hop le

Artifact bat buoc:

- `docs/day-20-order-state-machine.md`

### Ngay 21: Inventory transition

- [ ] Trace inventory reserve/release trong `OrderService`
- [ ] Ghi lai luc nao tru hang, luc nao tra hang
- [ ] Danh gia consistency giua order status va inventory status

Artifact bat buoc:

- `docs/day-21-inventory-transition.md`

### Ngay 22: Transaction boundary

- [ ] Xem tat ca method `@Transactional`
- [ ] Ghi lai boundary nao dang hop ly, boundary nao dang rong qua
- [ ] Viet note ve rollback risk

Artifact bat buoc:

- `docs/day-22-transaction-boundary-review.md`

### Ngay 23: Concurrency va race condition

- [ ] Liet ke toi thieu 5 race condition co the xay ra
- [ ] Phan tich double checkout, double confirm, refresh token song song
- [ ] Ghi lai cach optimistic/pessimistic locking co the giup gi

Artifact bat buoc:

- `docs/day-23-concurrency-risk-register.md`

### Ngay 24: Tong hop order domain

- [ ] Tong hop toan bo note tuan 4
- [ ] Viet danh gia tai sao `OrderService` hien tai manh nhung chua senior-grade
- [ ] Liet ke refactor candidates

Artifact bat buoc:

- `docs/week-04-order-domain-summary.md`

---

## Tuan 5: Testing strategy

Muc tieu tuan:

- Nhin test duoi goc do behavior va regression safety
- Lap duoc test matrix cho cac flow quan trong

### Ngay 25: Unit test review

- [ ] Doc `OrderServiceTest.java`
- [ ] Doc test trong `coreservice/src/test`
- [ ] Danh gia quality cua test theo goc nhin behavior

Artifact bat buoc:

- `docs/day-25-unit-test-review.md`

### Ngay 26: Missing business tests

- [ ] Liet ke test cases con thieu cho order
- [ ] Liet ke test cases con thieu cho auth
- [ ] Liet ke test cases con thieu cho permission/security

Artifact bat buoc:

- `docs/day-26-missing-business-tests.md`

### Ngay 27: Integration test strategy

- [ ] Hoc `@SpringBootTest`, `@WebMvcTest`, MockMvc
- [ ] Chon endpoint nao nen viet integration test truoc
- [ ] Viet test plan cho auth va order

Artifact bat buoc:

- `docs/day-27-integration-test-strategy.md`

### Ngay 28: Test data strategy

- [ ] Ghi lai can nhung data nao de test auth
- [ ] Ghi lai can nhung data nao de test order
- [ ] Danh gia co nen dung Testcontainers hay chua

Artifact bat buoc:

- `docs/day-28-test-data-strategy.md`

### Ngay 29: Regression-focused testing

- [ ] Viet checklist truoc khi refactor service lon
- [ ] Ghi lai nhung behavior nao bat buoc phai duoc khoa bang test
- [ ] Dinh nghia smoke test cho project nay

Artifact bat buoc:

- `docs/day-29-regression-safety-checklist.md`

### Ngay 30: Tong hop testing

- [ ] Tong hop toan bo test strategy
- [ ] Viet 1 tai lieu test matrix cap he thong

Artifact bat buoc:

- `docs/week-05-testing-summary.md`

---

## Tuan 6: Production mindset va hardening

Muc tieu tuan:

- Hieu project duoi goc nhin production
- Nam logging, config, health, readiness, migration safety

### Ngay 31: Logging review

- [ ] Tim tat ca diem log quan trong trong auth va order
- [ ] Phan loai security log, business log, error log
- [ ] Danh gia log nao thieu context

Artifact bat buoc:

- `docs/day-31-logging-review.md`

### Ngay 32: Observability basics

- [ ] Hoc health, readiness, liveness, metrics co ban
- [ ] Kiem tra project da co actuator hay chua
- [ ] Ghi lai nhung endpoint/chi so nen co

Artifact bat buoc:

- `docs/day-32-observability-basics.md`

### Ngay 33: Config va profile

- [ ] Tim cac file config hien co
- [ ] Liet ke bien nao nen tach theo `local`, `test`, `prod`
- [ ] Danh gia config nao dang hard-code

Artifact bat buoc:

- `docs/day-33-config-and-profile-plan.md`

### Ngay 34: Secret va security config hygiene

- [ ] Review cach quan ly secret, cookie config, CORS config
- [ ] Viet note nhung dieu khong nen commit vao repo
- [ ] Viet checklist secure config

Artifact bat buoc:

- `docs/day-34-secure-config-checklist.md`

### Ngay 35: Migration va release-safe thinking

- [ ] Hoc Flyway hoac Liquibase o muc can biet
- [ ] Viet quy trinh them cot, index, constraint an toan
- [ ] Ghi lai cach release thay doi DB ma it rui ro

Artifact bat buoc:

- `docs/day-35-db-migration-playbook.md`

### Ngay 36: Tong hop production readiness

- [ ] Tong hop risk production cua project hien tai
- [ ] Viet production readiness checklist cho project nay

Artifact bat buoc:

- `docs/week-06-production-readiness-summary.md`

---

## Tuan 7: Refactor va tu duy senior

Muc tieu tuan:

- Hoc cach de xuat refactor dung trong pham vi an toan
- Biet uu tien technical debt
- Viet duoc roadmap cai tien co ly do

### Ngay 37: Technical debt inventory

- [ ] Liet ke technical debt chinh cua auth
- [ ] Liet ke technical debt chinh cua order
- [ ] Xep hang theo muc `critical`, `important`, `later`

Artifact bat buoc:

- `docs/day-37-technical-debt-inventory.md`

### Ngay 38: OrderService refactor roadmap

- [ ] Viet roadmap tach `OrderService` theo tung buoc nho
- [ ] Moi buoc phai co muc tieu va regression risk rieng
- [ ] Khong de xuat refactor toan bo mot lan

Artifact bat buoc:

- `docs/day-38-order-service-refactor-roadmap.md`

### Ngay 39: Auth flow refactor roadmap

- [ ] Viet roadmap chuan hoa auth controller, error handling, cookie handling
- [ ] Ghi lai buoc nao nen lam truoc, buoc nao nen lam sau

Artifact bat buoc:

- `docs/day-39-auth-refactor-roadmap.md`

### Ngay 40: Code review checklist

- [ ] Viet checklist review code rieng cho Java Spring Boot project nay
- [ ] Checklist phai cover: transaction, validation, authz, logging, test, DB, naming

Artifact bat buoc:

- `docs/day-40-code-review-checklist.md`

### Ngay 41: Tradeoff writing

- [ ] Chon 3 quyet dinh ky thuat trong project
- [ ] Viet ly do chon A thay vi B
- [ ] Neu chua biet ly do, viet gia thuyet va risk

Artifact bat buoc:

- `docs/day-41-technical-tradeoffs.md`

### Ngay 42: Tong hop tuan 7

- [ ] Gom roadmap refactor, debt inventory, checklist review thanh 1 bo senior toolkit

Artifact bat buoc:

- `docs/week-07-senior-toolkit-summary.md`

---

## Tuan 8: Capstone senior-ready

Muc tieu tuan:

- Tong hop toan bo kien thuc da hoc
- Tao 1 case study nghiem tuc du de tu danh gia va dung khi phong van

### Ngay 43: Chon capstone

- [ ] Chon `Order Capstone` hoac `Auth Capstone`
- [ ] Viet problem statement va pham vi phan tich

Artifact bat buoc:

- `docs/day-43-capstone-scope.md`

### Ngay 44: Current design analysis

- [ ] Phan tich thiet ke hien tai cua flow da chon
- [ ] Liet ke diem manh, diem yeu, risk, coupling

Artifact bat buoc:

- `docs/day-44-current-design-analysis.md`

### Ngay 45: Test strategy cho capstone

- [ ] Viet test matrix cho capstone
- [ ] Xac dinh test nao bat buoc co truoc khi refactor

Artifact bat buoc:

- `docs/day-45-capstone-test-strategy.md`

### Ngay 46: Production va performance concerns

- [ ] Viet nhung concern ve logging, config, DB, performance, concurrency cua capstone

Artifact bat buoc:

- `docs/day-46-capstone-production-concerns.md`

### Ngay 47: Refactor roadmap cho capstone

- [ ] Viet roadmap cai tien theo tung phase nho
- [ ] Ghi ro expected outcome va risk reduction cua tung phase

Artifact bat buoc:

- `docs/day-47-capstone-refactor-roadmap.md`

### Ngay 48: Tong hop va tu danh gia

- [ ] Viet tai lieu capstone hoan chinh
- [ ] Tu cham diem minh theo 6 truc:
  - Spring Boot core
  - Security
  - JPA va transaction
  - Business design
  - Testing
  - Production mindset
- [ ] Liet ke 10 viec can tiep tuc de cham moc senior that su

Artifact bat buoc:

- `docs/week-08-capstone-summary.md`
- `docs/final-self-assessment.md`

---

## Tieu chi tu danh gia moi cuoi tuan

Moi cuoi tuan, tu tra loi 10 cau sau va cham 0-2 diem moi cau:

1. Toi co giai thich lai chu de tuan nay ma khong can nhin tai lieu khong?
2. Toi co gan duoc ly thuyet vao dung file/flow trong project khong?
3. Toi co nhin ra it nhat 3 risk ky thuat that su khong?
4. Toi co viet duoc artifact ro rang, ngan gon, dung trong tam khong?
5. Toi co phan biet duoc business rule va technical detail khong?
6. Toi co viet duoc test ideas co gia tri khong?
7. Toi co nhin ra tradeoff thay vi chi noi dung/sai khong?
8. Toi co hieu duoc transaction/data consistency o chu de tuan nay khong?
9. Toi co biet neu dua len production thi van de gi de phat sinh khong?
10. Toi co the noi chuyen ve chu de nay theo goc nhin senior-ready khong?

Moc diem:

- `0-8`: hoc chua vao, phai hoc lai
- `9-14`: da hieu co ban, chua sau
- `15-17`: da kha vung
- `18-20`: dat muc senior-ready cho chu de do

## Tieu chi cuoi 2 thang

Neu sau 48 ngay hoc, ban lam duoc cac dieu sau thi roadmap nay dat:

- Giai thich ro architecture cua `ecommerce-shop`
- Ve duoc auth flow va order state machine khong can nhin code
- Chi ra duoc query risk, transaction risk, concurrency risk
- Viet duoc testing strategy dung cho auth va order
- Viet duoc refactor roadmap cho `OrderService` va auth flow
- Co the tu code review duoi goc nhin senior ky thuat
- Co 1 capstone summary du manh de dung lam tai lieu on phong van

## Ghi chu cuoi cung

Neu ban muon tang toc hon nua, hay bien roadmap nay thanh 3 lop output:

1. Lop note hoc tap: moi ngay 1 file nho
2. Lop tong hop tuan: moi tuan 1 file summary
3. Lop capstone: 1 bo tai lieu ket thuc 2 thang

Khong can hoc them nhieu framework moi trong 2 thang nay. Chi can bam sat project hien tai, hoc sau, viet ro, va tu review nghiem tuc.
