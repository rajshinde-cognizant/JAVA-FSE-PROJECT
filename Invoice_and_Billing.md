# Invoice and Billing Service

## Table of Contents

- [Overview](#overview)
- [Component Diagram](#component-diagram)
- [Database Table Design](#database-table-design)
- [Endpoints](#endpoints)
- [Key Features](#key-features)
- [Sequence Diagram](#sequence-diagram)
- [Swagger Documentation](#swagger-documentation)
---


## Overview
- **Manages**: Invoices, payment statuses, and invoice downloads.
- **Provides**: RESTful endpoints for invoice generation, retrieval, status updates, and PDF downloads.
- **Communication**: Interacts with the Booking Service and Service Center Management Service via Feign Clients to fetch booking and service type details.
---
## Component Diagram

```mermaid
 graph TD

    style A fill:#f9f,stroke:#333,stroke-width:2px
    style B fill:#bbf,stroke:#333,stroke-width:2px
    style C fill:#bfb,stroke:#333,stroke-width:2px
    style D fill:#bff,stroke:#333,stroke-width:2px
    style E fill:#ffb,stroke:#333,stroke-width:2px
    style F fill:#fbb,stroke:#333,stroke-width:2px
    style G fill:#cfc,stroke:#333,stroke-width:2px
    style H fill:#ccf,stroke:#333,stroke-width:2px


    A[API Gateway] --> B[Invoice Service]
    B --> C[Invoice Controller]
    C --> D[Invoice Service Layer]
    D --> E[Invoice Repository]
    E --> F[H2 Database - Invoice]
    D -- Feign Client --> G[Booking Service]
    D -- Feign Client --> H[Service Center Service]

    subgraph Invoice Service
        C
        D
        E
    end
```
---

## Key Features
- **Invoice Generation**
    -Automatically create invoices based on bookings and selected service types.

- **Payment Tracking**
    - Update and monitor payment status (e.g., Paid, Pending).

- **PDF Export**
    - Download invoices in PDF format for record-keeping.

- **Service Integration**
    - Full CRUD operations for service centers, mechanics, and service types.

- **Microservice Ready**
    - Designed for scalability and integration in a distributed system.
 ---
 
## Database Table Design

#### Table: `Invoice`

| Field Name       | Data Type     | Description                              |
|------------------|---------------|------------------------------------------|
| `InvoiceID`      | `INT`         | Primary Key, unique identifier           |
| `BookingID`      | `INT`         | Foreign Key referencing `BookingID`      |
| `ServiceTypeID`  | `INT`         | Foreign Key referencing `ServiceTypeID`  |
| `TotalAmount`    | `DECIMAL(10,2)`| Total amount charged                     |
| `PaymentStatus`  | `VARCHAR(20)` | Status of the payment (e.g., Paid)       |

---

## Endpoints

| Method | Endpoint                                 | Description                          |
|--------|------------------------------------------|--------------------------------------|
| POST   | `/api/invoices`                          | Generate invoice for a booking       |
| GET    | `/api/invoices`                          | Get all invoices for a user          |
| GET    | `/api/invoices/{id}`                     | Get invoice details                  |
| PUT    | `/api/invoices/{id}/status`              | Update payment status                |
| GET    | `/api/invoices/{id}/download`            | Download invoice PDF                 |

---

## Sequence Diagram

```mermaid
 sequenceDiagram
    participant User
    participant API Gateway
    participant InvoiceService
    participant BookingService
    participant ServiceCenterService
    participant InvoiceDB

    User->>API Gateway: POST /api/invoices
    API Gateway->>InvoiceService: Route request
    InvoiceService->>BookingService: Fetch booking details
    InvoiceService->>ServiceCenterService: Fetch service type info
    InvoiceService->>InvoiceDB: Save invoice
    InvoiceDB-->>InvoiceService: Confirmation
    InvoiceService-->>API Gateway: Invoice created
    API Gateway-->>User: Invoice details 
```
---

## Swagger Documentation
The Invoice and Billing Service provides interactive API documentation using Swagger.

### Access Swagger UI
Swagger UI for User Service
    - http://localhost:8086/swagger-ui/index.html
