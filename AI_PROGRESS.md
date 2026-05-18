# AI Progress Log

File nay da duoc tach nho de AI co the doc on dinh hon.
Lich su chi tiet nam trong thu muc `AI_PROGRESS/` theo tung ngay.

## Cach ghi nhan

- Sau moi lan thay doi code hoac tai lieu, append mot muc moi vao file ngay hien tai trong `AI_PROGRESS/`.
- Moi muc can du de team frontend, backend, va AI khac tiep tuc theo doi.
- Khong xoa lich su cu. Neu mot todo khong con phu hop, danh dau trang thai thay vi xoa dong.
- Neu mot file theo ngay vuot qua kich thuoc doc thoai mai cho AI, tach tiep thanh `YYYY-MM-DD-part-1.md`, `YYYY-MM-DD-part-2.md`.

## Mau ghi nhan

### YYYY-MM-DD HH:mm - Project - Task title

- Scope: frontend | backend | shared
- Changed:
  - file/path-1
  - file/path-2
- Summary:
  - change 1
  - change 2
- Verification:
  - command/result
- Todo:
  - [ ] next step 1
  - [ ] next step 2
- Notes:
  - risk, blocker, handoff note

## Chi muc file

- `AI_PROGRESS/2026-04-16.md`
- `AI_PROGRESS/2026-04-17.md`
- `AI_PROGRESS/2026-04-21.md`
- `AI_PROGRESS/2026-04-22.md`
- `AI_PROGRESS/2026-04-19.md`
- `AI_PROGRESS/2026-04-18.md`
- `AI_PROGRESS/2026-04-20.md`
- `AI_PROGRESS/2026-04-23.md`
- `AI_PROGRESS/2026-04-24.md`
- `AI_PROGRESS/2026-04-26.md`
- `AI_PROGRESS/2026-04-27.md`
- `AI_PROGRESS/2026-04-28.md`
- `AI_PROGRESS/2026-04-29.md`
- `AI_PROGRESS/2026-05-02.md`
- `AI_PROGRESS/2026-05-08.md`
- `AI_PROGRESS/2026-05-09.md`
- `AI_PROGRESS/2026-05-10.md`
- `AI_PROGRESS/2026-05-18.md`

---

### 2026-05-18 15:25 - ecommerce-shop - Async checkout saga command consumers

- Scope: backend
- Log: `AI_PROGRESS/2026-05-18.md`
- Summary:
  - Added `SagaCommandConsumer` and `SagaCommandHandlerService` so checkout saga command topics are consumed and reply topics are published for orchestrator progression.
- Verification:
  - `basecommerce` targeted test `SagaCommandHandlerServiceTest` passed with 3 tests.

### 2026-05-08 14:00 - ecommerce-shop - Payment module full implementation

- Scope: backend
- Changed:
  - `basecommerce/src/main/java/com/ttl/base/payment/domain/PaymentIntent.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/domain/PaymentAttempt.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/domain/PaymentLedgerEntry.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/saga/CheckoutSaga.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/saga/CheckoutSagaStatus.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/saga/CheckoutSagaOrchestrator.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/saga/ProcessedMessage.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/service/PaymentService.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/idempotency/PaymentIdempotencyService.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/idempotency/PaymentIdempotencyRecord.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/outbox/PaymentOutboxEvent.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/outbox/PaymentOutboxPublisher.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/finance/ReconciliationService.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/finance/ChargebackService.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/finance/ChargebackCase.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/finance/ReconciliationResult.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/finance/ProviderStatementLine.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/controller/PaymentController.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/controller/FinanceController.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/config/PaymentKafkaConfig.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/repository/PaymentIntentRepository.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/repository/PaymentAttemptRepository.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/repository/PaymentLedgerRepository.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/saga/CheckoutSagaRepository.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/saga/ProcessedMessageRepository.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/idempotency/PaymentIdempotencyRepository.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/outbox/PaymentOutboxRepository.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/finance/ChargebackCaseRepository.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/finance/ReconciliationResultRepository.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/finance/ProviderStatementLineRepository.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/dto/request/CreateIntentRequest.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/dto/request/AuthorizeRequest.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/dto/request/CaptureRequest.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/dto/request/RefundRequest.java`
  - `basecommerce/src/main/java/com/ttl/base/payment/dto/response/PaymentIntentResponse.java`
  - `basecommerce/src/main/resources/db/migration/mysql/V20260508_01__payment_domain.sql`
  - `basecommerce/src/main/resources/db/migration/postgresql/V20260508_01__payment_domain.sql`
  - `basecommerce/src/test/java/com/ttl/base/payment/domain/PaymentIntentDomainTest.java`
  - `basecommerce/src/test/java/com/ttl/base/payment/saga/CheckoutSagaStateTest.java`
  - `common/src/main/java/com/ttl/common/dto/PaymentStatus.java`
  - `common/src/main/java/com/ttl/common/dto/PaymentOperation.java`
  - `common/src/main/java/com/ttl/common/constant/ITag.java`
- Summary:
  - Full payment module implementation following `docs/payment-roadmap/` (files 00–11), covering all 5 phases:
    1. Domain foundation — PaymentIntent entity with full state machine, PaymentAttempt audit log, PaymentLedgerEntry financial journal
    2. Reliability — PaymentIdempotencyService (SHA-256 payload hash + 24h TTL + 409 conflict), PaymentOutboxPublisher (SELECT FOR UPDATE SKIP LOCKED + exponential backoff + DLQ)
    3. Distributed workflow — CheckoutSagaOrchestrator (Reserve→Authorize→Confirm→Capture→Completed; compensation: void→release→cancel; dedup via ProcessedMessage; 3 retries before manual review)
    4. Finance operations — ReconciliationService (EOD match internal ledger vs provider statement + mismatch classification), ChargebackService (OPEN→UNDER_REVIEW→EVIDENCE→WON/LOST + ledger adjustment)
    5. REST API — PaymentController (intents CRUD + authorize/capture/refund/void) + FinanceController (reconcile endpoint)
  - Provider gateway is stub/mock; next is ProviderGatewayService with Resilience4j
- Verification:
  - Maven compile blocked by JRE-only environment; all code reviewed manually against existing patterns
- Todo:
  - [ ] Implement ProviderGatewayService with Resilience4j (circuit breaker + retry + timeout) per `docs/payment-roadmap/06-2pc-poc-production-hardening.md`
  - [ ] Add Micrometer payment metrics service per `docs/payment-roadmap/08-observability-runbook.md`
  - [ ] Wire CheckoutSagaOrchestrator into CartController checkout flow (publish OrderCreated → Kafka → saga consumer)
  - [ ] Run `mvnw.cmd compile -pl basecommerce,common` when JDK 17 is available
  - [ ] Write idempotency integration tests (same key + same payload → 200; same key + diff payload → 409)

### 2026-05-09 00:00 - ecommerce-shop - Java core multithreading file I/O assessment doc

- Scope: docs
- Changed:
  - `docs/java-core-multithreading-fileio-senior-assessment.md`
- Summary:
  - Viết self-assessment theo format `docs/experience-with-restful-apis-senior-assessment.md`, đối chiếu repo cho 25 chủ đề Java core/multithreading/file I/O, đánh dấu mức độ có/chưa có, kèm Evidence, Nhận xét, Gap senior interview, và Ví dụ hình dung cho từng mục.
- Verification:
  - Manual review markdown, đối chiếu file source/docs trong repo.
- Todo:
  - [ ] Bổ sung artifact cho JMM, lock, Atomic, BlockingQueue, ForkJoin, parallel file processing
  - [ ] Viết benchmark/load test report có số liệu p95/p99
  - [ ] Cân nhắc tách phần bonus thành tài liệu riêng nếu cần học sâu hơn

### 2026-05-09 00:00 - ecommerce-shop - Java core multithreading file I/O assessment doc

- Scope: docs
- Changed:
  - `docs/java-core-multithreading-fileio-senior-assessment.md`
- Summary:
  - Viết self-assessment theo format `docs/experience-with-restful-apis-senior-assessment.md`, đối chiếu repo cho 25 chủ đề Java core/multithreading/file I/O, đánh dấu mức độ có/chưa có, kèm Evidence, Nhận xét, Gap senior interview, và Ví dụ hình dung cho từng mục. Bổ sung code snippets vào phần Ví dụ hình dung để dễ hình dung từng pattern.
- Verification:
  - Manual review markdown, đối chiếu file source/docs trong repo.
- Todo:
  - [ ] Bổ sung artifact cho JMM, lock, Atomic, BlockingQueue, ForkJoin, parallel file processing
  - [ ] Viết benchmark/load test report có số liệu p95/p99
  - [ ] Cân nhắc tách phần bonus thành tài liệu riêng nếu cần học sâu hơn

### 2026-05-09 00:00 - ecommerce-shop - Java core multithreading file I/O assessment doc

- Scope: docs
- Changed:
  - `docs/java-core-multithreading-fileio-senior-assessment.md`
- Summary:
  - Thêm mục “Vì sao cần / nếu không có thì sao” vào từng chủ đề theo style self-assessment microservice, để mỗi topic có trade-off và failure mode rõ hơn ngoài Evidence/Gap/Ví dụ.
- Verification:
  - Manual review markdown.
- Todo:
  - [ ] Rà lại wording từng mục nếu muốn giảm lặp
  - [ ] Tách vài ví dụ code dài ra file riêng nếu tài liệu quá nặng
