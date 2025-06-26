
# Service Center Management Service

The Service Center Management Service is a core module of the Vehicle Management System. It handles the management of service centers, mechanics, and service types.

---

## Overview

- **Manages**: Service centers, mechanics, and service types.
- **Provides**: RESTful endpoints for CRUD operations.
- **Communication**: Can be queried by other services (e.g., Invoice Service).

---

## Architecture

```mermaid
graph TD
    A[API Gateway] --> B[Service Center Service]
    B --> C[Service Center Controller]
    C --> D[Service Center Service Layer]
    D --> E[Service Center Repository]
    E --> F[H2 Database - Service Center]

    subgraph Service Center Service
        C
        D
        E
    end
```

## Database Table Design

### Service Center Management Service

#### Table: `ServiceCenter`

| Field Name       | Data Type     | Description                              |
|------------------|---------------|------------------------------------------|
| `ServiceCenterID`| `INT`         | Primary Key, unique identifier           |
| `Name`           | `VARCHAR(100)`| Name of the service center               |
| `Location`       | `VARCHAR(255)`| Physical address                         |
| `Contact`        | `VARCHAR(50)` | Contact number or email                  |

#### Table: `Mechanic`

| Field Name       | Data Type     | Description                              |
|------------------|---------------|------------------------------------------|
| `MechanicID`     | `INT`         | Primary Key, unique identifier           |
| `ServiceCenterID`| `INT`         | Foreign Key referencing `ServiceCenterID`|
| `Name`           | `VARCHAR(100)`| Full name of the mechanic                |
| `Expertise`      | `VARCHAR(100)`| Area of specialization                   |

#### Table: `ServiceType`

| Field Name       | Data Type     | Description                              |
|------------------|---------------|------------------------------------------|
| `ServiceTypeID`  | `INT`         | Primary Key, unique identifier           |
| `Description`    | `TEXT`        | Description of the service               |
| `Price`          | `DECIMAL(10,2)`| Cost of the service                      |

---

## Endpoints

### Service Center Management Service

| Method | Endpoint                                 | Description                          |
|--------|------------------------------------------|--------------------------------------|
| POST   | `/api/service-centers`                   | Add a new service center             |
| GET    | `/api/service-centers`                   | List all service centers             |
| GET    | `/api/service-centers/{id}`              | Get service center details           |
| POST   | `/api/service-centers/{id}/mechanics`    | Add mechanic to a center             |
| GET    | `/api/service-centers/{id}/mechanics`    | List mechanics in a center           |
| POST   | `/api/service-types`                     | Define a new service type            |
| GET    | `/api/service-types`                     | List all service types               |

Swagger url - http://localhost:8085/swagger-ui/index.html#/student-controller/createStudent
## Sequence Diagrams

### Service Center Management Service

```mermaid
 sequenceDiagram
    participant Admin
    participant API Gateway
    participant ServiceCenterService
    participant ServiceCenterDB

    Admin->>API Gateway: POST /api/service-centers
    API Gateway->>ServiceCenterService: Route request
    ServiceCenterService->>ServiceCenterDB: Save service center
    ServiceCenterDB-->>ServiceCenterService: Confirmation
    ServiceCenterService-->>API Gateway: Response
    API Gateway-->>Admin: Service center added 
```
