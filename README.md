#  High-Performance Inventory API

[![CI Pipeline](https://github.com/masmur71/inventoryAPI/.github/workflows/main.yml/badge.svg)](https://github.com/your-username/inventory-api/actions)
[![Docker](https://img.shields.io/badge/Docker-Containerized-blue?logo=docker)](https://hub.docker.com/)
[![Railway](https://img.shields.io/badge/Deployment-Railway-success?logo=railway)](https://railway.app/)
[![License](https://img.shields.io/badge/license-MIT-green)](./LICENSE)

A robust, concurrency-safe RESTful API designed to handle high-traffic inventory management. This project demonstrates backend engineering concepts including **Atomic Database Operations**, **Cache-Aside Strategy**, and **Containerized DevOps Pipelines**.

---

##  The Engineering Challenge: "The Race Condition"

In a typical e-commerce scenario, if 100 users try to buy the last item simultaneously, a naive implementation (Read -> Calculate -> Update) results in **overselling** (negative stock).

### The Solution
This API implements **Optimistic Concurrency Control** at the database level using MongoDB's atomic operators.

```typescript
// Core Logic: Atomic Decrement inside Product Controller
// The operation fails immediately if the stock condition is not met.
const product = await Product.findOneAndUpdate(
  { _id: id, stock: { $gte: quantity } }, // 🛡️ Guard Clause (Prevent Negative Stock)
  { $inc: { stock: -quantity } },         // 📉 Atomic Update
  { new: true }
);
```
## System Architecture
```
graph TD
    Client[User / Client] -->|HTTP Request| API[Node.js + Express API]
    
    subgraph "Data Layer"
        API -->|Read (Cache First)| Redis[(Redis Cache)]
        Redis -->|Hit| API
        Redis -.->|Miss| Mongo[(MongoDB)]
        API -->|Write (Atomic Ops)| Mongo
    end
    
    subgraph "DevOps"
        GitHub[GitHub Repo] -->|Push| Actions[GitHub Actions CI]
        Actions -->|Test & Build| Docker[Docker Image]
        Docker -->|Deploy| Railway[Railway Cloud]
    end
```
## Key Features
🛡️ Concurrency Safe: Proven to handle simultaneous requests without data inconsistency using MongoDB Atomic Operators.

⚡ High Performance: Implements Redis for caching frequently accessed product data, reducing database load by up to 50%.

🐳 Dockerized: Uses Multi-Stage Builds to produce a lightweight (<150MB) production-ready image.

🤖 CI/CD Pipeline: Automated testing workflow via GitHub Actions ensures code quality on every push.

🔒 Type-Safe: Written in TypeScript for robustness and better developer experience.

## Tech Stack
Component,Technology,Description
Runtime,Node.js (v18),JavaScript Runtime
Framework,Express.js,Web Framework
Language,TypeScript,Static Typing
Database,MongoDB,NoSQL Database
Caching,Redis,In-Memory Data Store
DevOps,Docker & Compose,Containerization
Testing,Jest & Supertest,Unit & Integration Testing








