# 📊 Stock Market Insight Platform

A modular, backend-driven architecture for ingesting, transforming, and analyzing stock market data using Java 21, Spring Boot, Kafka, MongoDB, and PostgreSQL — wrapped in cloud-native goodness.

---

## 🔧 Modules Overview

### 1️⃣ `market-ingestor`
Ingests real-time stock data from public APIs and stores raw responses in MongoDB for high-speed storage and flexibility.

- ✅ Scheduled or event-driven fetch using Spring Scheduler or Kafka
- ✅ Uses Redis for rate-limiting, deduplication, or caching ticker data
- ✅ Publishes ingestion events to Kafka for downstream systems

### 2️⃣ `etl-transformer`
Transforms raw MongoDB documents into normalized PostgreSQL tables for analytics.

- ✅ Kafka consumer that listens for ingestion events
- ✅ ETL logic for schema mapping and enrichment
- ✅ Optional batch processing support via Spring Batch
- ✅ Redis for caching intermediate or reusable transformations

### 3️⃣ `analytics-engine`
Serves analytics, prediction endpoints, and dashboard data via REST APIs.

- ✅ Time-series trend analysis, watchlist scoring, and price alerts
- ✅ PostgreSQL-powered query layer with indexes, window functions
- ✅ Exposes monitoring endpoints via Spring Boot Actuator
- ✅ Integrates with visualization tools like Grafana or Metabase

---

## 🌐 Shared Infrastructure

All modules integrate with shared dev services using Docker Compose:

| Component        | Purpose                                        |
|------------------|------------------------------------------------|
| MongoDB          | Fast document-based storage (ingestion layer) |
| PostgreSQL       | Relational analytics DB                        |
| Kafka            | Event pipeline and service decoupling         |
| Redis            | Caching and rate-limiting                     |
| Spring Config    | Centralized configuration management           |
| ELK Stack        | Observability: logs, metrics, traces          |

---

