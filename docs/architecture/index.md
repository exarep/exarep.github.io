# Architecture Overview

Exarep follows a **microservices architecture** deployed on Red Hat OpenShift, with services communicating through REST APIs, gRPC, and event-driven messaging via Apache Kafka.

## High-Level Architecture

```mermaid
graph TB
    subgraph External
        ERCOT[ERCOT Market Systems]
        CUSTOMERS[Customers]
        STAFF[Internal Staff]
    end

    subgraph Portals
        CP[Customer Portal]
        IP[Internal Portal]
    end

    subgraph API Gateway
        GW[Red Hat Connectivity Link]
    end

    subgraph Services
        CS[Customer Service]
        ES[Enrollment Service]
        BS[Billing Service]
        US[Usage Service]
        MS[Market Service]
        PS[Product Service]
    end

    subgraph Messaging
        KAFKA[AMQ Streams / Kafka]
    end

    subgraph Data
        DB_CS[(Customer DB)]
        DB_ES[(Enrollment DB)]
        DB_BS[(Billing DB)]
        DB_US[(Usage DB)]
        DB_MS[(Market DB)]
        DB_PS[(Product DB)]
    end

    CUSTOMERS --> CP
    STAFF --> IP
    CP --> GW
    IP --> GW
    GW --> CS
    GW --> ES
    GW --> BS
    GW --> US
    GW --> PS

    CS --> DB_CS
    ES --> DB_ES
    BS --> DB_BS
    US --> DB_US
    MS --> DB_MS
    PS --> DB_PS

    ES --> KAFKA
    MS --> KAFKA
    US --> KAFKA
    BS --> KAFKA

    MS --> ERCOT
    ERCOT --> MS
```

## Design Principles

**Service Autonomy**
:   Each service owns its data and business logic. Services communicate through well-defined APIs and events, never sharing databases.

**API-First**
:   All service interfaces are defined using OpenAPI specifications for REST and Protocol Buffers for gRPC before implementation begins.

**Event-Driven**
:   Domain events are published to Kafka topics, enabling loose coupling between services and supporting eventual consistency.

**GitOps-Driven**
:   All cluster configuration and application deployments are managed declaratively through Git repositories and reconciled by Argo CD.

**Infrastructure as Code**
:   AWS infrastructure is provisioned with Terraform and configured with Ansible, ensuring repeatable and auditable environments.

## Communication Patterns

| Pattern              | Use Case                                                  | Technology          |
| -------------------- | --------------------------------------------------------- | ------------------- |
| Synchronous REST     | Portal-to-service calls, CRUD operations                  | Quarkus RESTEasy    |
| Synchronous gRPC     | High-performance inter-service calls                      | Quarkus gRPC        |
| Asynchronous Events  | Domain events, cross-service coordination                 | AMQ Streams (Kafka) |
| Batch Integration    | ERCOT market file processing                              | Scheduled jobs      |
