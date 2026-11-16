# Microservices

A Java 17 and Spring Boot project structured as a Maven multi-module build. It currently contains two services: `customer` (REST service for registering customers) and `fraud` (fraud detection service), both using PostgreSQL.

## Project structure

```text
.
├── customer/                 # Customer registration service
│   └── src/main/
│       ├── java/hamdy/       # Application, API, service, and persistence code
│       └── resources/        # Spring configuration and startup banner
├── fraud/                    # Fraud detection service
│   └── src/main/
│       ├── java/hamdy/       # Application, API, service, and persistence code
│       └── resources/        # Spring configuration
├── docker-compose.yaml       # PostgreSQL and pgAdmin for local development
└── pom.xml                   # Parent Maven project
```

## Prerequisites

- JDK 17
- Maven
- Docker with the Compose plugin

## Run locally

1. Start PostgreSQL and pgAdmin:

   ```sh
   docker compose up -d
   ```

2. Create the database used by the customer service. The Compose configuration creates a PostgreSQL database named `admin` by default, while the service connects to `customer`:

   ```sh
   docker compose exec postgres psql -U admin -c "CREATE DATABASE customer;"
   ```

3. Start the service:

   ```sh
   mvn -pl customer spring-boot:run
   ```

The service listens on port `8080`. PostgreSQL is exposed on port `5432`, and pgAdmin on port `5050`. The development database credentials are configured in `docker-compose.yaml` and `customer/src/main/resources/application.yaml`; change them before using this setup outside local development.

## Customer registration API

Send a `POST` request to `/api/v1/customers` with a JSON body:

```json
{
  "firstName": "Ada",
  "lastName": "Lovelace",
  "email": "ada@example.com"
}
```

## Build

Build and verify all Maven modules from the repository root:

```sh
mvn clean verify
```
