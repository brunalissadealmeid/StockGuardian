Backend – Stock Portfolio Simulator + Anomaly Detector

Project Overview

This backend system simulates a stock portfolio with:

- A RESTful API for managing financial assets.
- NoSQL storage using MongoDB.
- Kafka event-driven anomaly detection.
- Integration with Yahoo Finance and Alpha Vantage APIs.
- Dockerized architecture for rapid deployment.

---

Technical Goals

- Create a REST API to simulate and manage stock investments.
- Persist dynamic asset data in MongoDB.
- Detect statistical anomalies in stock behavior.
- Orchestrate services with Docker and Kafka.

---

Technologies

- **Java 17**
- **Spring Boot (Web, Data MongoDB, Kafka)**
- **MongoDB**
- **Apache Kafka**
- **Yahoo Finance + Alpha Vantage APIs**
- **Docker & Docker Compose**
- **Swagger / OpenAPI**
- **JUnit 5 + Mockito**

---

Project Structure

src/main/java/com/yourproject/
├── controller # REST API endpoints
├── service # Business logic
├── repository # MongoDB access
├── model # Data models (MongoDB documents)
├── dto # Data transfer objects
├── config # Kafka, Mongo, Swagger config
└── utils # Helper classes


---

REST API – Main Endpoints

| Method | Endpoint               | Description |
|--------|------------------------|-------------|
| `POST` | `/api/stocks`          | Add a new stock to the portfolio |
| `GET`  | `/api/portfolio`       | Retrieve portfolio summary |
| `GET`  | `/api/anomalies`       | Get anomalies by ticker |
| `PUT`  | `/api/stocks/{ticker}` | Update stock quantity or price |
| `DELETE` | `/api/stocks/{ticker}` | Remove stock from portfolio |

>  Swagger documentation available at:  
> `http://localhost:8080/swagger-ui.html`

---

MongoDB Data Model

```json
{
  "_id": "64e8fae...",
  "ticker": "AAPL",
  "quantity": 100,
  "purchasePrice": 145.50,
  "currentPrice": 152.35,
  "profit": 685.00,
  "anomalies": [
    {
      "date": "2025-07-14",
      "type": "spike",
      "description": "Sharp 12% increase in price"
    }
  ]
}

Anomaly Detection
Statistical deviation check (e.g. Z-Score)

Spike and pattern break detection

Kafka events are triggered when anomalies are found

MongoDB is updated via Kafka consumers

Kafka Integration
java
Copiar
Editar
@KafkaListener(topics = "anomalies", groupId = "portfolio-group")
public void consume(String jsonMessage) {
    // Parse and update stock with anomaly info
}
Producer: emits anomaly events.

Consumer: processes events and updates MongoDB.

Docker Setup
docker-compose.yml
yaml
Copiar
Editar
version: '3.8'
services:
  mongo:
    image: mongo
    ports:
      - "27017:27017"

  kafka:
    image: bitnami/kafka
    ports:
      - "9092:9092"
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181

  zookeeper:
    image: bitnami/zookeeper
    ports:
      - "2181:2181"

  backend:
    build: .
    ports:
      - "8080:8080"
    depends_on:
      - kafka
      - mongo
Run the app
bash
Copiar
Editar
docker-compose up --build

Testing
Unit tests using JUnit 5 + Mockito

Service and repository layer coverage

Integration tests with TestContainers for Kafka and MongoDB (optional)

Future Improvements
Spring Security + JWT auth

WebSocket for real-time portfolio updates

Scheduled sync with APIs (Quartz/Spring Task)

Monitoring with Spring Actuator + Prometheus

Author
Bruna Almeida – Full Stack Developer
GitHub | LinkedIn

License
This project is licensed under the MIT License – see LICENSE for details.
