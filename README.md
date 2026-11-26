# study-nest-fastify-adapter

> A high-throughput NestJS 9 microservice configured with FastifyAdapter and Pino logger for low-overhead HTTP request execution.

**Inception (First Commit):** `2022-12-29` • **Batch / Horizon:** `batch-2021-2022` • **Architecture Domain:** `High-Throughput Backend / NestJS Fastify Adapter`

---

## 📌 Architecture & Overview

`nest-fastify` is an architectural proof-of-concept demonstrating how to replace NestJS's default Express HTTP engine with Fastify via `@nestjs/platform-fastify`. By substituting Express with Fastify's schema-compiled serializer and event loop optimizations, the service achieves significantly lower latency and up to two times higher request-per-second throughput under heavy concurrent workloads.

The application configures Fastify's native logger (`new FastifyAdapter({ logger: true })`) and binds to all network interfaces (`0.0.0.0`) to facilitate Docker containerization and Kubernetes cluster ingress routing.

```
                              +-----------------------------------------------+
                              |                 nest-fastify                  |
                              |                 (Port: 3000)                  |
                              |                                               |
[ HTTP Client / Ingress ]     |  +--------------------+                       |
       |                      |  | Fastify Logger     |                       |
       | HTTP GET /           |  | (High-Perf Pino)   |                       |
       v                      |  +---------+----------+                       |
+---------------------------+ |            |                                  |
| Fastify HTTP Engine       +-+->+---------v----------+                       |
| (FastifyAdapter 0.0.0.0)  | |  | NestJS IoC Kernel  |                       |
+---------------------------+ |  | (AppModule)        |                       |
                              |  +---------+----------+                       |
                              |            |                                  |
                              |            v                                  |
                              |  +--------------------+                       |
                              |  | AppController      |                       |
                              |  | (GET /)            |                       |
                              |  +---------+----------+                       |
                              |            |                                  |
                              |            v                                  |
                              |  +--------------------+                       |
                              |  | AppService         |                       |
                              |  | (getHello)         |                       |
                              |  +--------------------+                       |
                              +-----------------------------------------------+
```

* **Context:** Performance benchmarking prototype exploring Fastify HTTP adapter integration, low-allocation request processing, and container-ready socket binding.
* **Key Patterns:**
  * **Platform Agnostic Inversion:** Swapping HTTP engines at bootstrapping without altering core NestJS controller or service decorator logic.
  * **High-Throughput Adapter Integration:** Leveraging Fastify's internal JSON schema compiler and radix-tree router to maximize I/O efficiency.
  * **Zero-Allocation Logging:** Integrating Fastify's native Pino JSON logger for minimal CPU overhead during request tracing.

---

## 🛠 Tech Stack

* **Framework & Core:** NestJS 9.0 (`@nestjs/core`, `@nestjs/common`), `@nestjs/platform-fastify` 9.2.1
* **HTTP Engine:** Fastify
* **Language & Compiler:** TypeScript 4.7.4, `@nestjs/cli` 9.0.0
* **Testing:** Jest 29.3.1, Supertest 6.1.3, `ts-jest` 29.0.3

---

## ✨ Features & Endpoints

* **High-Performance HTTP Adapter:** Fastify runtime replacing Express for reduced request latency and elevated concurrency limits.
* **Integrated Request Tracing:** Automated JSON request/response logging powered by Fastify logger.
* **Container-Ready Binding:** Configured for container deployments with `0.0.0.0` host interface binding.
* **Multi-Tier Testing Suite:** Preconfigured Jest unit and end-to-end (E2E) testing configurations.

### Key API Routes / Interfaces

| Method | Route | Description |
|---|---|---|
| `GET` | `/` | Health and basic diagnostic endpoint returning string greeting. |

---

## 🚀 Getting Started

### Prerequisites

* Node.js `>= 16.0.0`
* npm `>= 8.0.0`

### Installation

```bash
# Clone the repository
git clone https://github.com/fabogit/study-nest-fastify-adapter.git
cd study-nest-fastify-adapter

# Install dependencies
npm install
```

### Running the Application

```bash
# Start in development mode with hot-reload (listens on port 3000)
npm run start:dev

# Build for production
npm run build
npm run start:prod
```

### Running Tests

```bash
# Unit tests
npm run test

# End-to-end (E2E) tests
npm run test:e2e

# Test coverage report
npm run test:cov
```

---

## 📄 License & Author

Developed by [@fabogit](https://github.com/fabogit).
