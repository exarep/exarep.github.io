# Development Overview

This section covers everything needed to develop, test, and contribute to the Exarep microservices.

## Development Environment

| Tool      | Purpose                         | Version            |
| --------- | ------------------------------- | ------------------ |
| Java      | Application development         | Latest LTS         |
| Maven     | Build tool                      | Latest             |
| Node.js   | Frontend development            | Latest LTS         |
| Angular   | Frontend framework              | Latest LTS         |
| Podman    | Container runtime               | Latest             |
| Git       | Version control                 | Latest             |
| Python    | Documentation and automation    | Latest             |

## Repository Structure

Each API service follows a consistent project layout:

```
api-{service}/
├── src/
│   ├── main/
│   │   ├── java/com/exarep/{service}/
│   │   │   ├── api/
│   │   │   ├── service/
│   │   │   └── repository/
│   │   └── resources/
│   │       ├── application.yaml
│   │       ├── ValidationMessages.properties
│   │       └── db/
│   │           ├── migration/
│   │           │   └── V1__initial_schema.sql
│   │           └── testdata/
│   │               └── V999__testdata.sql
│   └── test/
│       └── java/com/exarep/{service}/
│           └── api/
│               └── {Entity}ResourceTest.java
├── Containerfile
├── pom.xml
└── README.md
```

## Local Development

Quarkus dev services automatically provision local databases, Kafka brokers, and Keycloak instances during development. No manual setup of external dependencies is required.

Start any service in dev mode:

```bash
cd api-{service}
./mvnw quarkus:dev
```

The service will be available at `http://localhost:8080` with live reload enabled.

## Testing

Every service includes comprehensive API endpoint tests. Tests run against Quarkus test infrastructure with dev services providing all dependencies.

```bash
./mvnw test
```

Test data is loaded from `src/main/resources/db/testdata/V999__testdata.sql` during dev and test profiles. Primary key values in test data are lower than the sequence starting value to allow easy cleanup.
