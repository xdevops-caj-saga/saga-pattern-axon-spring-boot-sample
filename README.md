[TOC]

# Saga Pattern Implementation with Axon and Spring Boot



## Related Posts

- [Part 1](https://progressivecoder.com/saga-pattern-implementation-with-axon-and-spring-boot-part-1/)
- [Part 2](https://progressivecoder.com/saga-pattern-implementation-axon-spring-boot-part-2/)
- [Part 3](https://progressivecoder.com/saga-pattern-implementation-axon-spring-boot-part-3/)
- [Part 4](https://progressivecoder.com/saga-pattern-implementation-with-axon-and-spring-boot-part-4/)



## Commands and Events

| #    | Command                  | Event               |
| ---- | ------------------------ | ------------------- |
| 1    | CreateOrderCommand       | OrderCreatedEvent   |
| 2    | CreateInvoiceCommand     | InvoiceCreatedEvent |
| 3    | CreateShippingCommand    | OrderShippedEvent   |
| 4    | UpdateOrderStatusCommand | OrderUpdatedEvent   |



## Test

### Start Axon Server

Use *podman* to run Axon Server by its Docker image:
```bash
podman run -d --name axonserver -p 8024:8024 -p 8124:8124 axoniq/axonserver
```

Access Axon Server dashboard: <http://localhost:8024>

### Start Services

Start each service in an individual terminal:

Start Order service (default port is `8080`):
```bash
mvn clean package spring-boot:run -pl order-service --also-make
```

Start Payment service (default port is `8081`):
```bash
mvn clean package spring-boot:run -pl payment-service --also-make
```

Start Shipping service (default port is `8082`):
```bash
mvn clean package spring-boot:run -pl shipping-service --also-make
```

### Verify on Axon Server dashboard

On Axon Server dashboard:
- Open "Overview" to see service typology
- Open "Command" to see commands


### Test the Saga

Access Swagger UI <http://localhost:8080/swagger-ui.html>

Create an order to start a Saga as below:

API:

```bash
POST /api/orders
```

Request body:

```json
{
  "currency": "USD",
  "itemType": "SMARTPHONE",
  "price": 799
}
```

Notes: The `itemType` must matches in `ItemType` enum class (`LAPTOP`, `HEADPHONE`, `SMARTPHONE`).

### Verify Saga execution

On Axon Server dashboard:
- Open "Command" page to see which service issue which commands
- Open "Search" page to see published events