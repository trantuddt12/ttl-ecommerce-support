# Kafka-Based E-commerce System Design

## 1. Muc tieu

Tai lieu nay tong hop thiet ke tong quat cho he thong e-commerce su dung Kafka lam event backbone. Muc tieu la cung cap mot blueprint gan production de co the chia nho thanh tung component va trien khai dan vao project.

Pham vi tai lieu bao gom:
- Cart
- Catalog
- Promotion/Pricing
- Order
- Payment
- Inventory
- Shipment
- Refund
- Notification
- Search sync
- Analytics pipeline
- Outbox / CDC
- Retry / DLQ
- Monitoring / Observability
- Schema governance

## 2. Kien truc tong the

```text
                               +----------------------+
                               |   Web / Mobile App   |
                               +----------+-----------+
                                          |
                                          v
                               +----------------------+
                               |     API Gateway      |
                               | Auth / Rate Limit    |
                               +----------+-----------+
                                          |
      --------------------------------------------------------------------------------
      |                |                 |                |                 |
      v                v                 v                v                 v
+------------+   +-------------+   +-------------+  +-------------+  +--------------+
| Cart Svc   |   | Catalog Svc |   | Promotion   |  | User Svc    |  | Search Svc   |
| Redis/DB   |   | Product DB  |   | Pricing     |  | Auth/Profile|  | OpenSearch   |
+------+-----+   +------+------+   +------+------+  +------+------+  +------+-------+
       |                |                 |                |                  ^
       | outbox         | outbox          | outbox         |                  |
       v                v                 v                |                  |
+-------------+   +-------------+   +-------------+       |                  |
| Outbox Tbl  |   | Outbox Tbl  |   | Outbox Tbl  |       |                  |
+------+------+   +------+------+   +------+------+       |                  |
       \______________   |   _____________/               |                  |
                      \  |  /                            |                  |
                       v v v                             |                  |
              +--------------------------+               |                  |
              | Outbox Publisher / CDC   |               |                  |
              +-----------+--------------+               |                  |
                          |                              |                  |
                          v                              |                  |
=========================== KAFKA CLUSTER =======================================
 Core Topics:
 - order.events
 - payment.events
 - inventory.events
 - shipment.events
 - refund.events
 - cart.events
 - catalog.events
 - promotion.events

 Infra Topics:
 - retry.*
 - dlq.*
 - audit.events
 - schema.changes
=================================================================================

        |                    |                    |                     |
        v                    v                    v                     v
+-------------+    +---------------+    +---------------+    +------------------+
| Order Svc   |    | Payment Svc   |    | Inventory Svc |    | Notification Svc |
+------+------+    +-------+-------+    +-------+-------+    +--------+---------+
       |                     |                    |                     |
       | outbox              | outbox             | outbox              | retry/dlq
       v                     v                    v                     v
+-------------+     +-------------+      +-------------+      +------------------+
| Outbox Tbl  |     | Outbox Tbl  |      | Outbox Tbl  |      | retry.notification|
+------+------+     +------+------+      +------+------+      | dlq.notification  |
       |                     |                    |           +------------------+
       +----------+----------+--------------------+
                  |
                  v
               +------+
               |Kafka |
               +------+
                  |
   -------------------------------------------------------------------------
   |                         |                      |                       |
   v                         v                      v                       v
+---------------+   +----------------+   +--------------------+   +------------------+
| Shipment Svc  |   | Refund Svc     |   | Saga Orchestrator  |   | Fraud/Risk Svc  |
+-------+-------+   +-------+--------+   | (or choreography)  |   +--------+---------+
        |                   |            +----------+---------+            |
        | outbox            | outbox                |                      |
        v                   v                       v                      |
+-------------+     +-------------+        +-------------------+           |
| Outbox Tbl  |     | Outbox Tbl  |        | Compensation Flow |<----------+
+------+------+     +------+------+        | - refund          |
       |                   |               | - release stock   |
       +---------+---------+               | - cancel order    |
                 |                         +-------------------+
                 v
              +------+
              |Kafka |
              +------+

Data Layer
+------------------------------------------------------------------+
| Kafka Connect                                                    |
| - Sink: S3 / Snowflake / BigQuery                                |
| - Source: DB CDC (Debezium)                                      |
+------------------------------------------------------------------+

Observability & Control
+------------------------------------------------------------------+
| Monitoring Stack                                                 |
| - Prometheus + Grafana (lag, throughput, error rate)            |
| - ELK / OpenSearch (logs)                                       |
| - Tracing (Jaeger/Tempo)                                        |
| - Alerting (DLQ spike, consumer lag, payment failure rate)      |
|                                                                  |
| Admin / Ops Tools                                               |
| - DLQ viewer + replay tool                                      |
| - Event audit timeline (theo orderId)                           |
| - Feature flag / config service                                 |
+------------------------------------------------------------------+

Schema & Governance
+-----------------------------+
| Schema Registry             |
| - Avro / Protobuf schema    |
| - Versioning / compatibility|
+-----------------------------+
```

## 3. Cac thanh phan chinh

### 3.1 API Gateway

Trach nhiem:
- xac thuc va phan quyen
- rate limit
- routing vao cac service ben duoi
- co the gop them request logging va trace context

### 3.2 Cart Service

Trach nhiem:
- luu gio hang hien tai cua user
- cap nhat so luong, xoa item, ap voucher preview
- tao du lieu checkout snapshot truoc khi tao order

Luu tru:
- Redis cho truy cap nhanh
- DB neu can persist lau dai

Event goi y:
- `cart.updated`
- `cart.checked_out`

### 3.3 Catalog Service

Trach nhiem:
- quan ly san pham, SKU, category, brand, media, attribute
- phat tan thay doi de search index va cac service khac dong bo

Event goi y:
- `product.created`
- `product.updated`
- `product.disabled`

### 3.4 Promotion Service

Trach nhiem:
- tinh gia, coupon, voucher, flash sale
- giu logic pricing tap trung
- cung cap ket qua pricing cho cart va order

Event goi y:
- `promotion.created`
- `promotion.updated`
- `pricing.rule.changed`

### 3.5 Order Service

Trach nhiem:
- tao order tu checkout snapshot
- giu order state machine
- phat event business cho cac service con lai

Trang thai goi y:
- `PENDING_PAYMENT`
- `PAID`
- `RESERVED`
- `SHIPPING`
- `COMPLETED`
- `FAILED`
- `REFUNDED`

Event goi y:
- `order.created`
- `order.confirmed`
- `order.failed`
- `order.cancelled`

### 3.6 Payment Service

Trach nhiem:
- tao payment intent
- capture thanh toan
- hoan tien khi can compensation

Event goi y:
- `payment.requested`
- `payment.completed`
- `payment.failed`
- `payment.refund_requested`
- `payment.refund_completed`

### 3.7 Inventory Service

Trach nhiem:
- reserve ton kho
- deduct ton kho sau khi xac nhan
- release reservation neu flow fail

Event goi y:
- `inventory.reserved`
- `inventory.failed`
- `inventory.released`

### 3.8 Shipment Service

Trach nhiem:
- tao shipment
- dong bo trang thai van chuyen
- ket noi nha van chuyen ngoai

Event goi y:
- `shipment.created`
- `shipment.dispatched`
- `shipment.delivered`
- `shipment.failed`

### 3.9 Refund Service

Trach nhiem:
- xu ly refund khi inventory fail, cancel order, return
- theo doi refund lifecycle

Event goi y:
- `refund.created`
- `refund.completed`
- `refund.failed`

### 3.10 Notification Service

Trach nhiem:
- gui email, SMS, push
- consume event business va gui thong bao bat dong bo

Can co:
- retry co gioi han
- DLQ cho cac truong hop khong gui duoc

### 3.11 Search Sync

Trach nhiem:
- consume event tu Catalog
- dong bo du lieu sang OpenSearch/Elasticsearch

Luu y:
- Search khong nen doc truc tiep DB catalog de phuc vu query lon

### 3.12 Analytics Pipeline

Trach nhiem:
- consume cac event business de tinh dashboard, BI, funnel, GMV
- day du lieu sang DWH/lake qua Kafka Connect hoac stream processor

## 4. Event backbone va topics

### 4.1 Core topics

- `cart.events`
- `catalog.events`
- `promotion.events`
- `order.events`
- `payment.events`
- `inventory.events`
- `shipment.events`
- `refund.events`
- `notification.events`
- `search.sync`
- `analytics.events`

### 4.2 Infra topics

- `retry.order`
- `retry.payment`
- `retry.inventory`
- `retry.notification`
- `dlq.order`
- `dlq.payment`
- `dlq.inventory`
- `dlq.notification`
- `audit.events`

### 4.3 Partition key goi y

- `orderId` cho order/payment/inventory/shipment/refund
- `productId` cho catalog/search sync
- `sku` cho inventory neu can theo tung SKU
- `userId` cho cart va user activity

Nguyen tac:
- tat ca event cua cung mot order nen vao cung mot partition de giu ordering theo order do

## 5. Message schema tong quat

```json
{
  "eventId": "evt-10001",
  "eventType": "ORDER_CREATED",
  "aggregateId": "ord-9001",
  "traceId": "trc-abc-001",
  "schemaVersion": 1,
  "occurredAt": "2026-04-22T13:10:00Z",
  "payload": {
    "orderId": "ord-9001",
    "userId": "u-123",
    "totalAmount": 22000000
  }
}
```

Field quan trong:
- `eventId`: de idempotency va audit
- `eventType`: loai su kien business
- `aggregateId`: key nghiep vu chinh, thuong la `orderId`
- `traceId`: lien ket tracing xuyen service
- `schemaVersion`: version contract
- `occurredAt`: timestamp event

## 6. Outbox / CDC

Outbox la thanh phan gan nhu bat buoc neu service vua ghi DB vua phat event.

### 6.1 Van de can giai quyet

Neu he thong lam kieu:
1. ghi DB thanh cong
2. publish Kafka that bai

Thi se gay lech trang thai:
- business data da ton tai
- nhung downstream khong nhan duoc event

### 6.2 Cach lam de xuat

Moi service co:
- business table
- outbox table

Trong cung transaction:
- ghi business data
- ghi 1 outbox record

Sau do:
- Outbox Publisher hoac Debezium doc outbox
- publish len Kafka

### 6.3 Outbox table goi y

Cot co ban:
- `id`
- `aggregate_id`
- `event_type`
- `payload`
- `status`
- `created_at`
- `published_at`

## 7. Luong nghiep vu chinh

### 7.1 Happy path

1. User cap nhat gio hang trong `Cart Service`
2. `Promotion Service` tinh discount/voucher
3. User checkout
4. `Order Service` tao order, ghi DB va outbox
5. Event `order.created` duoc day len Kafka
6. `Payment Service` consume va xu ly thanh toan
7. Publish `payment.completed`
8. `Inventory Service` consume, reserve ton kho
9. Publish `inventory.reserved`
10. `Shipment Service` consume, tao shipment
11. Publish `shipment.created`
12. `Notification Service` gui thong bao
13. `Analytics Pipeline` cap nhat dashboard

### 7.2 Failure path

Vi du thanh toan thanh cong nhung het hang:

1. `Payment Service` publish `payment.completed`
2. `Inventory Service` fail va publish `inventory.failed`
3. `Saga Orchestrator` hoac `Order Service` kich hoat compensation
4. `Refund Service` tao refund
5. Publish `refund.created`
6. Payment gateway xac nhan refund
7. Publish `refund.completed`
8. Order duoc cap nhat sang `FAILED` hoac `REFUNDED`
9. `Notification Service` gui thong bao xin loi

## 8. Saga va compensation

Khi flow business trai dai qua nhieu service, can mot co che de dam bao ket qua nghiep vu cuoi cung dung.

Hai huong pho bien:
- choreography: moi service tu consume/publish event va tu phan ung
- orchestration: mot service workflow dieu phoi luong xu ly

De xuat thuc te:
- bat dau bang choreography neu he thong con nho
- tach `Saga Orchestrator` rieng khi flow compensation phuc tap hon

Compensation thuong gom:
- refund payment
- release inventory reservation
- cancel order
- thong bao cho user va support

## 9. Retry / DLQ

### 9.1 Nguyen tac

Consumer khong duoc fail roi bo message.

Can co flow:
1. xu ly that bai
2. dua vao `retry.*`
3. retry theo so lan va backoff gioi han
4. neu van fail thi dua vao `dlq.*`

### 9.2 Ung dung

Rat can cho:
- notification provider bi timeout
- payment gateway loi tam thoi
- service downstream tam unavailable
- schema mismatch

### 9.3 Van hanh

Can co tool:
- xem DLQ messages
- tim theo `orderId` hoac `eventId`
- replay co kiem soat sau khi fix loi

## 10. Idempotency

Kafka consumer co the nhan lai message do retry hoac rebalance. Moi service xu ly side effect phai idempotent.

Vi du:
- khong duoc charge tien 2 lan
- khong duoc tru ton kho 2 lan
- khong duoc gui 10 email cho cung 1 event

De xuat:
- luu `eventId` da xu ly
- check truoc khi xu ly side effect
- voi payment/refund can co unique business key

## 11. Monitoring va observability

### 11.1 Metrics

Can co dashboard cho:
- throughput theo topic
- consumer lag theo group
- error rate theo service
- retry rate
- DLQ volume
- payment success/failure rate

### 11.2 Logging

Log tap trung can co:
- `traceId`
- `orderId`
- `eventId`
- service name
- consumer group

### 11.3 Tracing

Distributed tracing can theo doi duoc mot order di qua:
- Order
- Payment
- Inventory
- Shipment
- Refund
- Notification

### 11.4 Alerting

Can canh bao khi:
- consumer lag vuot nguong
- DLQ tang dot bien
- publish error tang cao
- broker co van de suc khoe
- payment failure rate bat thuong

## 12. Schema governance

De xuat dung:
- Avro hoac Protobuf neu can contract chat
- JSON Schema neu muon linh hoat hon

Can co:
- Schema Registry
- versioning ro rang
- compatibility check truoc deploy

Nguyen tac:
- khong thay doi pha vo backward compatibility neu co the tranh
- event moi thi tang version hoac them event type moi tuy convention

## 13. Data va analytics

### 13.1 Search sync

Catalog event -> Search sync -> OpenSearch/Elasticsearch

Muc tieu:
- query nhanh
- ho tro search, filter, sort, suggestion

### 13.2 BI / DWH

Kafka -> Kafka Connect / Streams -> S3 / Snowflake / BigQuery

Muc tieu:
- bao cao BI
- business dashboard
- machine learning / recommendation

## 14. Danh sach implementation uu tien

### Phase 1: Toi thieu de chay flow order

- API Gateway
- Cart Service
- Promotion Service
- Order Service
- Payment Service
- Inventory Service
- Notification Service
- Kafka Cluster
- Outbox cho Order/Payment/Inventory

### Phase 2: Hoan thien consistency va van hanh

- Retry topics
- DLQ topics
- idempotent consumer
- metrics + logging + tracing
- basic admin tool xem event timeline

### Phase 3: Mo rong production

- Shipment Service
- Refund Service
- Saga Orchestrator
- Search sync
- Analytics pipeline
- Schema Registry
- DLQ replay tool

## 15. Checklist production-ready

- moi service business co outbox hoac CDC
- event contract co version ro rang
- consumer co idempotency
- co retry va DLQ
- co compensation flow cho order fail giua chung
- co dashboard consumer lag va DLQ
- co tracing theo `orderId`/`traceId`
- co tool xem audit timeline
- co partition key dung theo domain

## 16. Ket luan

Kien truc nay phu hop cho he thong e-commerce event-driven, trong do Kafka dong vai tro event backbone, con Outbox/CDC, Retry/DLQ, Saga, Monitoring va Schema Registry giup he thong du tin cay de tien gan toi production.

Tai lieu nay nen duoc dung nhu ban thiet ke tong quat. Tu day co the tach thanh tai lieu rieng cho tung component de trien khai:
- `cart-service.md`
- `catalog-service.md`
- `promotion-service.md`
- `order-service.md`
- `payment-service.md`
- `inventory-service.md`
- `shipment-service.md`
- `refund-service.md`
- `notification-service.md`
- `event-topics.md`
- `outbox-cdc.md`
- `retry-dlq.md`
- `monitoring-observability.md`
- `schema-governance.md`
