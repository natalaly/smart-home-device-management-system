# Smart Home Tech Platform

A comprehensive microservices-based platform for smart home management and e-commerce operations, built with Spring Boot and event-driven architecture.

## Table of Contents

- [Project Overview](#project-overview)
- [Architecture](#architecture)
  - [Infrastructure Services](#1-infrastructure-services-infra)
  - [Telemetry Module](#2-telemetry-module-telemetry)
  - [Commerce Module](#3-commerce-module-commerce)
- [Technology Stack](#technology-stack)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Infrastructure Setup](#infrastructure-setup)
  - [Running the Services](#running-the-services)
- [Kafka Topics](#kafka-topics)
- [Testing](#testing)


## Project Overview

This project implements a distributed system for managing smart home devices and providing e-commerce functionality for smart home products. The system is built using microservices architecture with Spring Cloud components and leverages Apache Kafka for event-driven communication.

## Architecture

The project consists of three main modules:

### 1. Infrastructure Services (`infra`)
Core infrastructure components that support the microservices ecosystem:

| Service | Description | Technology |
|---------|-------------|------------|
| **[Config Server](infra/config-server)** | Centralized configuration management | Spring Cloud Config |
| **[Discovery Server](infra/discovery-server)** | Service registry and discovery | Netflix Eureka |
| **[Gateway](infra/gateway)** | API Gateway for routing and load balancing | Spring Cloud Gateway |

### 2. Telemetry Module (`telemetry`)
IoT telemetry processing system for smart home devices:

| Service | Description | Protocol |
|---------|-------------|----------|
| **[Collector](telemetry/collector)** | Collects sensor events and hub events | gRPC |
| **[Aggregator](telemetry/aggregator)** | Aggregates sensor data and creates snapshots | Kafka Streams |
| **[Analyzer](telemetry/analyzer)** | Analyzes telemetry data and triggers scenario actions | Kafka |
| **[Serialization](telemetry/serialization)** | Schema definitions for data serialization | Avro & Protobuf |
| └─ **[avro-schemas](telemetry/serialization/avro-schemas)** | Avro schema definitions for Kafka messages | Apache Avro |
| └─ **[proto-schemas](telemetry/serialization/proto-schemas)** | Protocol Buffer definitions for gRPC services | Protobuf |

### 3. Commerce Module (`commerce`)
E-commerce platform for smart home products:

| Service | Description | Communication |
|---------|-------------|---------------|
| **[Shopping Store](commerce/shopping-store)** | Product catalog management | REST API |
| **[Shopping Cart](commerce/shopping-cart)** | User shopping cart operations | REST + Feign |
| **[Warehouse](commerce/warehouse)** | Inventory management | REST API |
| **[Order](commerce/order)** | Order processing and management | REST + Feign |
| **[Payment](commerce/payment)** | Payment processing | REST API |
| **[Delivery](commerce/delivery)** | Delivery management | REST API |
| **[Interaction API](commerce/interaction-api)** | Shared DTOs and Feign clients | Library |

## Technology Stack

### Core Technologies
| Technology | Version | Purpose |
|------------|---------|---------|
| [Java](https://www.oracle.com/java/) | 21 | Primary programming language |
| [Spring Boot](https://spring.io/projects/spring-boot) | 3.3.2 | Application framework |
| [Spring Cloud](https://spring.io/projects/spring-cloud) | 2023.0.3 | Microservices infrastructure |
| [Maven](https://maven.apache.org/) | 3.8+ | Build and dependency management |

### Communication & Messaging
| Technology | Version | Purpose |
|------------|---------|---------|
| [Apache Kafka](https://kafka.apache.org/) | 3.6.1 | Event streaming platform |
| [gRPC](https://grpc.io/) | 1.63.0 | High-performance RPC framework |
| [OpenFeign](https://spring.io/projects/spring-cloud-openfeign) | - | Declarative REST client |

### Data Serialization
| Technology | Version | Purpose |
|------------|---------|---------|
| [Apache Avro](https://avro.apache.org/) | 1.11.3 | Data serialization for Kafka |
| [Protocol Buffers](https://protobuf.dev/) | 3.23.4 | Data serialization for gRPC |

### Infrastructure
| Technology | Version | Purpose |
|------------|---------|---------|
| [Netflix Eureka](https://github.com/Netflix/eureka) | - | Service discovery |
| [PostgreSQL](https://www.postgresql.org/) | 16.1 | Relational database |
| [Docker Compose](https://docs.docker.com/compose/) | - | Container orchestration |

### Documentation & Testing
| Technology | Version | Purpose |
|------------|---------|---------|
| [SpringDoc OpenAPI](https://springdoc.org/) | 2.6.0 | API documentation |
| [Postman](https://www.postman.com/) | - | API testing collection |

## Getting Started

### Prerequisites
- Java 21 or higher
- Maven 3.8+
- Docker and Docker Compose
- PostgreSQL 16.1 (or use Docker Compose)

### Infrastructure Setup

1. **Start Infrastructure Services**
   ```bash
   docker-compose up -d
   ```
   This will start:
   - Kafka broker (port 9092)
   - PostgreSQL database (port 5433)
   - Kafka topics initialization

2. **Build the Project**
   ```bash
   mvn clean install
   ```

### Running the Services

Start services in the following order:

1. **Config Server** (port 8888)
   ```bash
   cd infra/config-server
   mvn spring-boot:run
   ```

2. **Discovery Server** (port 8761)
   ```bash
   cd infra/discovery-server
   mvn spring-boot:run
   ```

3. **Gateway** (port configured via Config Server)
   ```bash
   cd infra/gateway
   mvn spring-boot:run
   ```

4. **Application Services**
   
   Telemetry Services:
   ```bash
   cd telemetry/collector && mvn spring-boot:run
   cd telemetry/aggregator && mvn spring-boot:run
   cd telemetry/analyzer && mvn spring-boot:run
   ```

   Commerce Services:
   ```bash
   cd commerce/shopping-store && mvn spring-boot:run
   cd commerce/shopping-cart && mvn spring-boot:run
   cd commerce/warehouse && mvn spring-boot:run
   cd commerce/order && mvn spring-boot:run
   cd commerce/payment && mvn spring-boot:run
   cd commerce/delivery && mvn spring-boot:run
   ```

## Kafka Topics

The system uses the following Kafka topics:

| Topic Name | Description | Purpose |
|------------|-------------|---------|
| `telemetry.sensors.v1` | Sensor events from smart home devices | Real-time sensor data streaming |
| `telemetry.snapshots.v1` | Aggregated sensor snapshots | Historical data aggregation |
| `telemetry.hubs.v1` | Hub lifecycle events | Device management events |

### Testing
Use the Postman collection provided in the [`postman/postman.json`](postman/postman.json) directory to test the APIs. The collection includes pre-configured requests for all commerce services.


**[⬆ Back to Top](#smart-home-tech-platform)**


