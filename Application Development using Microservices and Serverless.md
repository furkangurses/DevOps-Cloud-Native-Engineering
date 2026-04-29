# Application Development using Microservices and Serverless on OpenShift

This repository contains my hands-on labs, configuration files, and key takeaways from the **"Application Development using Microservices and Serverless on OpenShift"** course, offered by IBM on Coursera.

## 🚀 Course Overview
This course focuses on the transition from monolithic to microservices architecture, emphasizing cloud-native development principles. It covers the lifecycle of building, deploying, and managing applications using modern tools like Docker, Kubernetes, OpenShift, and Istio.

## 🛠 Key Skills & Technologies
- **Microservices Design:** Implementation of the 12-Factor App methodology and design patterns.
- **API Development:** Creating RESTful services using Python and Flask, documented with Swagger, and exploring GraphQL.
- **Serverless Computing:** Leveraging Function-as-a-Service (FaaS) and IBM Cloud Code Engine for auto-scaling workloads.
- **OpenShift & Istio:** Managing containerized applications on enterprise Kubernetes and implementing Service Mesh for traffic control and security.
- **DevOps Integration:** Streamlining microservices through CI/CD pipelines and cloud-native ecosystems.

## 📁 Repository Structure
The repository is organized by course modules:

1. **Introduction to Microservices:** Understanding architectural trade-offs and best practices.
2. **Web APIs:** Hands-on development of RESTful endpoints and API testing.
3. **Serverless Overview:** Exploring infrastructure-free code execution.
4. **Deployment:** Running microservices on IBM Cloud Code Engine.
5. **OpenShift & Istio:** Utilizing enterprise-grade orchestration and service mesh capabilities.
6. **Final Assignment:** End-to-end deployment of a cloud-native application.

---

## 📝 Lab Notes & Documentation
Each module folder contains specific `README.md` files detailing:
* YAML configuration files for deployment.
* CLI commands (oc, kubectl, curl) used during labs.
* Key architectural diagrams and logic.

---


# Cloud-Native Architecture: Twelve-Factor Apps & Microservices Engineering

This repository serves as a comprehensive technical guide and reference for building, deploying, and operating modern Software-as-a-Service (SaaS) applications. It bridges the gap between theoretical microservices patterns and production-grade DevOps implementation.

---

## 1. The Twelve-Factor App Methodology
The Twelve-Factor methodology is a set of best practices for building declarative, portable, and scalable SaaS applications. These factors are categorized into the three phases of the Software Delivery Lifecycle (SDLC).

### Phase I: Code
| Factor | Principle | DevOps Implementation |
| :--- | :--- | :--- |
| **I. Codebase** | One codebase, one app, multiple deploys. | Version control (Git) with distinct environments (Dev/Staging/Prod) via branching/tags. |
| **II. Dependencies** | Explicitly declare and isolate dependencies. | Use manifests (e.g., `package.json`, `requirements.txt`, `Go modules`) and containerization (Docker). |
| **V. Build, Release, Run** | Strict separation of stages. | CI/CD pipelines: Compile (Build) -> Config Injection (Release) -> Execution (Run). |
| **X. Dev/Prod Parity** | Keep development, staging, and production as similar as possible. | Infrastructure as Code (Terraform) and consistent container images across all stages. |

### Phase II: Deploy
* **III. Config:** Store configuration in the environment. Never hardcode credentials. Use Environment Variables or Secret Managers (HashiCorp Vault, K8s Secrets).
* **IV. Backing Services:** Treat resources (DBs, Message Queues) as attached resources. Swap a local MySQL for an AWS RDS instance via connection string changes only.
* **VI. Processes:** Execute the app as one or more stateless processes. Persistent data belongs in a backing service (e.g., Redis, PostgreSQL).
* **VII. Port Binding:** Export services via port binding. The app is self-contained and doesn't rely on injection of a webserver (e.g., using Gunicorn or Go’s `http` package).

### Phase III: Operate
* **VIII. Concurrency:** Scale out via the process model. Use horizontal scaling (spinning up more Pods/Containers) rather than vertical scaling.
* **IX. Disposability:** Maximize robustness with fast startup and graceful shutdown (handling `SIGTERM` for connection draining).
* **XI. Logs:** Treat logs as event streams. Standard Out (`stdout`) is the destination; let the infrastructure (ELK Stack, Splunk) handle aggregation.
* **XII. Admin Processes:** Run admin/management tasks (DB migrations, one-off scripts) as one-off processes against the identical release environment.

---

## 2. Microservices Architecture & Scaling
Microservices involve decomposing a single application into loosely coupled, independently deployable services.

### Core Characteristics
- **Technological Agnostic:** Each service uses the stack best suited for its domain (e.g., Python for AI, Go for high-throughput APIs).
- **Bounded Context:** Services are segregated by business logic, not technical layers.
- **Inter-service Communication:** Synchronous (REST, gRPC) or Asynchronous (Event Streaming via Kafka, RabbitMQ).

### The Scaling Model
> "Microservices enable precise resource allocation."

**Horizontal Scaling (Scaling Out):** Instead of upgrading a server's RAM (Vertical), we add more instances of a specific service.
* **Event-Driven Scaling:** Using Message Brokers to establish state changes and broadcast events, allowing consumers to scale based on queue depth.

---

## 3. Architectural Evolution: Monolith vs. SOA vs. Microservices

| Feature | Monolith | SOA (Service Oriented) | Microservices |
| :--- | :--- | :--- | :--- |
| **Scope** | Single Process | Enterprise-wide | Application-scoped |
| **Data Storage** | Shared Database | Shared/Integrated | Private per service (Database-per-service) |
| **Dependency** | Tightly Coupled | Discrete Functionality | Loosely Coupled |
| **Communication** | Method Calls | Enterprise Service Bus (ESB) | Dumb Pipes (APIs / Events) |
| **Complexity** | Low (at start) | High (due to orchestration) | High (due to distribution) |

---

## 4. Design Patterns for Distributed Systems

### ⚡ Front-End Patterns
- **Single Page Application (SPA):** Moves UI logic to the client side, interacting with backend APIs dynamically without page reloads.
- **Backend for Frontend (BFF):** A specialized microservice layer that tailors data for specific clients (Mobile vs. Web). This optimizes performance by reducing payload size for mobile networks.

### 🔄 Migration Patterns
- **Strangler Fig Pattern:** The industry standard for migrating monoliths. You "strangle" the old system by intercepting requests and routing them to new microservices one domain at a time until the monolith is obsolete.
    1. **Transform:** Build new functionality in a cloud-native env.
    2. **Coexist:** Run both side-by-side using a Reverse Proxy (Nginx/Envoy).
    3. **Eliminate:** Decommission the old code once traffic is fully diverted.

### 🌐 Management Patterns
- **Service Discovery:** In dynamic environments (Kubernetes), IP addresses change constantly. Service discovery (Consul, Kube-DNS) allows services to find each other automatically.
- **Adapter Pattern:** Translates incompatible interfaces, crucial when integrating legacy systems or 3rd party APIs into a new microservice ecosystem.

---

## 5. Engineering Guardrails: Anti-Patterns
To avoid "Distributed Monoliths," engineers must avoid these common pitfalls:

1.  **Premature Decomposition:** Don't start with microservices. Start with a clean monolith; move to microservices only when the complexity of the monolith hinders delivery.
2.  **Neglecting Automation:** Microservices without a robust CI/CD and automated observability (Prometheus/Grafana) will lead to operational failure.
3.  **Nanoservices:** Decomposing services too granularly creates excessive network overhead and management complexity. If deployment becomes "too hard," your services are likely too small.
4.  **SOA Rebirth:** If your microservices share a single massive database, you have created a SOA-style dependency, losing the benefit of independent scaling.
5.  **Gateway Neglect:** Avoid implementing Auth/Rate Limiting in every service. Use an **API Gateway** (Kong, Apigee) as a centralized entry point for non-functional requirements.

---



# Module 1: Microservices Foundations & API Documentation

This module establishes the foundational principles of modern software delivery, focusing on the Twelve-Factor App methodology, Microservices architecture patterns, and hands-on API lifecycle management using Swagger/OpenAPI.

---

## 🛠 Hands-on Lab: Modernizing JPetStore with Swagger UI
In modern engineering, an API is only as good as its documentation. This lab demonstrates how to containerize and utilize Swagger UI to interact with a RESTful service.

### 🐳 Containerized Documentation Setup
To visually render and interact with the OpenAPI (Swagger) specification, we deploy the UI as a standalone container:

```bash
# Pull the official Swagger UI image
docker pull swaggerapi/swagger-ui

# Deploy the UI on port 8080 (Mapping Host 8080 -> Container 8080)
docker run -dp 8080:8080 swaggerapi/swagger-ui

```

### 🧪 API Endpoint Lifecycle Testing

The following sequence simulates a standard CRUD operations flow for an inventory microservice:

1.  **POST /pet**: Create a new resource.
    
    -   _Payload:_ Specific JSON object defining `id`, `category`, and `status`.
        
    -   _Validation:_ Server Response Code **200 OK**.
        
2.  **GET /pet/{petId}**: Verify resource persistence.
    
    -   _Check:_ Validate that the state matches the POST payload.
        
3.  **POST /pet/{petId}**: Partial update (Update status to `sold`).
    
    -   _DevOps Note:_ This simulates a state change event in a transactional system.
        
4.  **DELETE /pet/{petId}**: Resource decommissioning.
    
    -   _Post-Condition:_ A subsequent `GET` must return **404 Not Found**, confirming successful deletion from the underlying data store.
        

----------

## 📘 Microservices & Modern SDLC Core Concepts

> **Executive Summary:** Modern software is delivered as **SaaS (Software as a Service)**. The 12-Factor methodology provides the blueprint for efficiency, while Microservices provide the scalability.

### Key Architectural Tenets

-   **Decoupling:** Each component is its own service, communicating via standardized API contracts.
    
-   **Polyglot Programming:** Services are technology-agnostic; teams choose the best stack for the specific business context.
    
-   **Independent Scalability:** Precision scaling of high-load components (Horizontal Scaling) without duplicating the entire application stack.
    
-   **Resilience:** Failures are isolated (Fault Tolerance). A crash in the 'Analytics' service does not bring down the 'Order Processing' service.
    

----------

## 📖 Engineering Glossary

**Term**

**Engineering Context**

**SaaS**

Centrally hosted software accessed via the internet (Web Apps).

**Build**

An immutable, executable unit created from source code.

**Release**

The combination of a **Build** and environment-specific **Configuration**.

**Run Stage**

The execution of a release in a target environment (Runtime).

**Horizontal Scaling**

"Scaling Out" by adding more instances (e.g., more Kubernetes Pods).

**Service Discovery**

Automated detection of dynamic network locations for microservices.

**BFF (Backend for Frontend)**

Specialized backend layer optimized for specific clients (Mobile vs. Desktop).

**Strangler Pattern**

Incremental migration strategy to replace legacy monoliths domain by domain.

**TLS**

Transport Layer Security; essential for securing service-to-service communication.

----------

## 🚦 Phase Mapping: The 12 Factors in the SDLC

### 1. Code Phase

-   **Factor 1 (Codebase):** Single Git repo per app; multiple deployments.
    
-   **Factor 2 (Dependencies):** Explicit declaration (no implicit system tools).
    
-   **Factor 5 (Build, Release, Run):** Strict separation of the deployment pipeline.
    
-   **Factor 10 (Dev/Prod Parity):** Minimizing gaps between local and production environments.
    

### 2. Deploy Phase

-   **Factor 3 (Config):** Use **Environment Variables** (Secret injection) for environment-specific data.
    
-   **Factor 4 (Backing Services):** DBs/Messaging Queues are attached resources accessible via URL.
    
-   **Factor 6 (Processes):** Stateless and "share-nothing" architecture.
    
-   **Factor 7 (Port Binding):** Self-contained services that export HTTP by binding to a port.
    

### 3. Operate Phase

-   **Factor 8 (Concurrency):** Scaling out via the process model.
    
-   **Factor 9 (Disposability):** Fast startup and graceful shutdown for robustness.
    
-   **Factor 11 (Logs):** Treat logs as event streams (Stdout/Stderr).
    
-   **Factor 12 (Admin Processes):** Run one-off management tasks (Migrations) against the release.

---



# Module 2: RESTful Architecture & API Gateway Integration

This module explores the backbone of distributed systems: **RESTful APIs** and **API Gateways**. It transitions from theoretical microservices concepts to the practical implementation of network communication protocols and centralized traffic management using Python and Flask.

---

## 📑 Table of Contents
1. [REST Architecture Standards](#1-rest-architecture-standards)
2. [The API Gateway Pattern](#2-the-api-gateway-pattern)
3. [Enterprise Gateway Solutions](#3-enterprise-gateway-solutions)
4. [Hands-on Lab: Provisioning a Micro Web Service](#4-hands-on-lab-provisioning-a-micro-web-service)

---

## 1. REST Architecture Standards

**REST (Representational State Transfer)** is the industry-standard architectural style for designing networked applications. In a cloud-native ecosystem, REST APIs act as the "dumb pipes" connecting our decoupled microservices.

### Core Principles of REST
* **Statelessness:** The golden rule of horizontal scalability. The server stores *no* client context between requests. Every HTTP request must contain all necessary authentication and payload data. This allows seamless load balancing across multiple backend pods.
* **Uniform Interface:** Resources are identified by specific URIs (Uniform Resource Identifiers). The API should decouple the internal database architecture from the representation sent to the client (usually JSON).
* **Client-Server Separation:** The UI/Frontend and the backend data storage are entirely independent, allowing them to evolve without breaking the contract.

### HTTP Verb Mapping (CRUD Operations)
In a production environment (e.g., a Financial Market Data Service), HTTP methods strictly map to CRUD operations:

| HTTP Method | CRUD Operation | Idempotent? | Example Endpoint | System Action |
| :--- | :--- | :--- | :--- | :--- |
| `POST` | **C**reate | No | `/api/v1/assets` | Provisions a new tracked commodity/asset. |
| `GET` | **R**ead | Yes | `/api/v1/assets/gold` | Retrieves current market data for Gold. |
| `PUT` | **U**pdate | Yes | `/api/v1/assets/gold` | Overwrites the existing asset configuration. |
| `DELETE`| **D**elete | Yes | `/api/v1/assets/gold` | Decommissions the asset from tracking. |

---

## 2. The API Gateway Pattern

In a distributed microservices environment, exposing raw internal services directly to clients creates massive security and coupling issues. The **API Gateway** pattern introduces a centralized reverse proxy that sits between external clients and your internal Kubernetes cluster/mesh.

> **Architectural Note:** The API Gateway is the "Front Door" of your infrastructure. It abstracts the internal microservice partitioning from the consumer.

### Strategic Benefits
- **Security & Edge Protection:** Centralized TLS termination, JWT validation, and DDoS protection via rate limiting.
- **Traffic Routing & Aggregation:** Reduces client round-trips. A single client request to `/api/v1/dashboard` can be fanned out by the gateway to the `Auth`, `Inventory`, and `Market-Data` services, returning a unified JSON response.
- **Observability:** Central point for metrics, logging, and tracing (e.g., exporting telemetry to Prometheus/Grafana).
- **Monetization:** Facilitates billing systems by tracking API quota usage per API Key.

### Engineering Trade-offs (Drawbacks)
- **Single Point of Failure (SPOF):** If the Gateway goes down, the entire system is unreachable. *Mitigation: Deploy in highly available (HA) clusters across multiple Availability Zones.*
- **Latency Overhead:** Adds an extra network hop.
- **Maintenance Complexity:** Requires dedicated configuration management (Infrastructure as Code).

---

## 3. Enterprise Gateway Solutions

Depending on the cloud strategy, engineering teams select from managed or open-source solutions:

| Category | Industry Tools | Use Case |
| :--- | :--- | :--- |
| **Open Source (Self-Hosted)** | **Kong**, Apache APISIX, Tyk, Gloo | High customization, multi-cloud setups, Kubernetes-native ingress. |
| **Managed Cloud (PaaS)** | AWS API Gateway, Google Apigee, Azure API Management | Rapid provisioning, zero infrastructure maintenance. |
| **Enterprise Appliance** | IBM DataPower Gateway | High-security, legacy enterprise integration. |

---

## 4. Hands-on Lab: Provisioning a Micro Web Service

In this lab, we bypass the simplistic "Hello World" to provision a baseline structure for a **Financial Market Data API** using Python and Flask. Flask is a micro-framework favored in DevOps and Data Engineering for its lightweight, unopinionated nature, making it perfect for containerized environments.

### Step 1: Environment Setup
Initialize your workspace and install the required WSGI framework.

```bash
# Create the working directory and enter it
mkdir -p /opt/market-data-api && cd /opt/market-data-api

# Install Flask using the Python package manager
python3 -m pip install flask

```

### Step 2: Application Code (`api.py`)

Instead of a raw string return, we will implement a proper JSON response standard for REST APIs. Create a file named `api.py`.

Python

```
# api.py
from flask import Flask, jsonify

# Initialize the Flask application
app = Flask(__name__)

# Mock Data Store (In production, this would query a backend DB or message broker)
COMMODITY_DATA = {
    "status": "active",
    "assets": [
        {"id": "XAU", "name": "Gold", "price_usd": 2350.40, "trend": "bullish"},
        {"id": "XAG", "name": "Silver", "price_usd": 28.15, "trend": "stable"}
    ]
}

# Define the REST endpoint
@app.route("/api/v1/commodities", methods=["GET"])
def get_commodities():
    """
    Retrieves the current status of tracked commodities.
    Returns HTTP 200 OK with a JSON payload.
    """
    # jsonify converts the Python dictionary to a valid JSON response with correct headers
    return jsonify(COMMODITY_DATA), 200

# Server execution block
if __name__ == "__main__":
    # In production, never use debug=True or the built-in server. 
    # You would use a production WSGI server like Gunicorn: `gunicorn -w 4 -b 0.0.0.0:5000 api:app`
    print("Starting Market Data microservice on port 5000...")
    app.run(host="0.0.0.0", port=5000, debug=True)

```

### Step 3: Execution and Verification

Run the microservice locally to bind it to port `5000`.

Bash

```
# Start the Flask server
python3 api.py

```

**Verification:** Open a separate terminal and test the REST endpoint using `curl` to verify the JSON payload and HTTP status codes.

Bash

```
curl -i -X GET http://localhost:5000/api/v1/commodities

```

_Expected Output:_

HTTP

```
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 172

{
  "assets": [
    {"id": "XAU", "name": "Gold", "price_usd": 2350.4, "trend": "bullish"},
    {"id": "XAG", "name": "Silver", "price_usd": 28.15, "trend": "stable"}
  ],
  "status": "active"
}
```

---


# REST API Automation & Lifecycle Management

As a DevOps Engineer, interacting with, testing, and building APIs is a daily requirement. This module transitions from understanding REST architecture to actively testing endpoints using industry-standard tools (`cURL`, Postman) and developing a state-management CRUD API using Python and Flask.

---

## 🛠 1. The API Interaction Toolkit

When managing distributed systems, you need reliable tools to verify network paths, debug latency, and validate JSON payloads. 

### CLI Automation: cURL (Client URL)
Developed in 1998, `curl` remains the undisputed standard for command-line HTTP transactions, automated shell scripts, and CI/CD pipeline health checks.

| Flag | Purpose | DevOps Use Case |
| :--- | :--- | :--- |
| `-X` | Specifies the HTTP method (GET, POST, PUT, DELETE). | Explicitly defining state-change requests. |
| `-H` | Passes custom headers to the server. | Injecting `Authorization: Bearer <token>` or `Content-Type: application/json`. |
| `-d` | Sends data (payload) in the request body. | Provisioning new infrastructure via API. |
| `-i` | Includes HTTP response headers in the output. | Debugging CORS issues or Cache-Control headers. |
| `-v` | Verbose mode. | Tracing the full TLS handshake and connection lifecycle. |

### Visual API Lifecycle: Postman
While `curl` is excellent for automated scripts, Postman is the enterprise standard for API development, offering:
* **Collections & Environments:** Manage Dev/Staging/Prod API variables seamlessly.
* **Automated Testing:** Write pre-request scripts and assertions (e.g., verifying a 200 OK status).
* **Collaboration:** Share workspaces with front-end developers to establish API contracts.

---

## 🏗️ 2. Hands-on Lab: Building a Microservice Registry API

*Industrial Context:* Instead of a generic "product store", we will build a **Microservice Deployment Registry**. This simulates a backend service that tracks active pods or services in a cluster.

### Step 2.1: Environment Initialization
Ensure you are in the correct directory and install the necessary Web Server Gateway Interface (WSGI) libraries.

```bash
# Navigate to workspace
cd /opt/infrastructure/registry-api

# Install Flask and CORS support for cross-origin UI requests
python3 -m pip install flask flask_cors

```

### Step 2.2: Application Architecture (`registry.py`)

Create the Flask application. Notice the strict mapping of HTTP verbs to CRUD operations.

Python

```
from flask import Flask, jsonify, request
import json

app = Flask("Microservice Registry API")

# In-memory datastore (Simulating a NoSQL DB or etcd key-value store)
# Tracking deployment configurations
deployments = [
    {'id': 101, 'service_name': 'auth-gateway', 'replicas': 3, 'status': 'Healthy'},
    {'id': 102, 'service_name': 'payment-processor', 'replicas': 2, 'status': 'Degraded'}
]

# [READ] Retrieve the full registry state
@app.route('/deployments', methods=['GET'])
def get_all_deployments():
    return jsonify(deployments), 200

# [READ] Retrieve a specific microservice configuration by ID
@app.route('/deployments/<int:id>', methods=['GET'])
def get_deployment(id):
    svc = next((item for item in deployments if item["id"] == id), None)
    if not svc:
        return jsonify({"error": "Service not found"}), 404
    return jsonify(svc), 200

# [CREATE] Register a new microservice deployment
@app.route('/deployments', methods=['POST'])
def register_deployment():
    payload = request.get_json()
    deployments.append(payload)
    # 201 Created is the standard REST response for successful POSTs
    return jsonify({"message": "Deployment registered successfully"}), 201

# [UPDATE] Patch/Update a deployment configuration (e.g., scaling replicas)
@app.route('/deployments/<int:id>', methods=['PUT'])
def update_deployment(id):
    updated_data = request.get_json()
    svc = next((item for item in deployments if item["id"] == id), None)
    
    if not svc:
        return jsonify({"error": "Service not found"}), 404
        
    for key, value in updated_data.items():
        svc[key] = value
        
    # 204 No Content or 200 OK are acceptable for PUT
    return jsonify({"message": "Configuration updated"}), 200

# [DELETE] Decommission a microservice
@app.route('/deployments/<int:id>', methods=['DELETE'])
def decommission_deployment(id):
    global deployments
    deployments = [item for item in deployments if item["id"] != id]
    return jsonify({"message": "Deployment decommissioned"}), 200

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5000)

```

----------

## ⚡ 3. Automated API Validation via cURL

Start the server in a background terminal: `python3 registry.py`. In a new terminal pane, execute the following integration tests using `curl`:

**1. Verify Initial State (GET)**

Bash

```
curl -s -X GET http://localhost:5000/deployments | jq .

```

**2. Provision a New Service (POST)**

Bash

```
curl -X POST http://localhost:5000/deployments \
    -H "Content-Type: application/json" \
    -d '{"id": 103, "service_name": "inventory-db", "replicas": 1, "status": "Provisioning"}'

```

**3. Verify the Provisioning (GET by ID)**

Bash

```
curl -s http://localhost:5000/deployments/103 | jq .

```

----------

## ✅ 4. Postman QA Validation Checklist

For complex nested JSON payloads or OAuth token generation, shift from CLI to Postman. Follow this QA checklist to ensure API compliance:

-   [ ] **Configure the Environment:** Set a `{{base_url}}` variable to `http://localhost:5000` to avoid hardcoding localhost.
    
-   [ ] **Test PUT (Scaling out):** * Select `PUT` -> `{{base_url}}/deployments/101`
    
    -   Body -> `raw` -> `JSON`
        
    -   Payload: `{"replicas": 5}` (Simulating a scale-out event).
        
    -   _Assert:_ Status code is `200 OK`.
        
-   [ ] **Test DELETE (Decommissioning):**
    
    -   Select `DELETE` -> `{{base_url}}/deployments/102`
        
    -   _Assert:_ Status code is `200 OK`.
        
-   [ ] **Test Error Handling:**
    
    -   Select `GET` -> `{{base_url}}/deployments/999`
        
    -   _Assert:_ System gracefully handles the exception and returns a `404 Not Found`.
        

> **DevOps Best Practice:** Always validate that your `PUT` and `DELETE` operations are idempotent. Sending the same DELETE request twice should not crash the server; it should simply confirm the resource is no longer there.

---


# 🚀 Enterprise Microservices: REST API Documentation & Testing with Swagger

> **Overview**: This repository provides a comprehensive blueprint for building, documenting, and testing RESTful microservices using Python, Flask, and Docker. It leverages the **OpenAPI Specification** and **Swagger UI** to deliver machine-readable, interactive, and language-agnostic API documentation.

![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)
![Flask](https://img.shields.io/badge/Flask-Microframework-lightgrey.svg)
![Swagger](https://img.shields.io/badge/Swagger-OpenAPI_3.0-85EA2D.svg)
![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED.svg)

---

## 📑 Table of Contents
- [Architecture & Core Concepts](#-architecture--core-concepts)
- [Prerequisites](#-prerequisites)
- [Local Development Setup](#-local-development-setup)
- [Docker Deployment: Cloud Status API](#-docker-deployment-cloud-status-api)
- [API Documentation (Swagger UI)](#-api-documentation-swagger-ui)
- [Glossary](#-glossary)

---

## 🏗️ Architecture & Core Concepts

APIs specify an interface and connect different systems, providing them with consistent communication. In a DevOps and Cloud environment, APIs must be robust, self-documenting, and scalable. 

This project demonstrates the following architectural principles:
- **Statelessness & Scalability**: REST APIs are stateless; each request contains all the information required to process it, making it highly scalable.
- **Uniform Interface**: Uses standard HTTP methods (`GET`, `POST`, `PUT`, `DELETE`) for CRUD operations.
- **API Gateway Integration**: Designed to sit behind an API Gateway, which acts as the front door for backend services, providing unified access and traffic management.
- **OpenAPI Standard**: A language-agnostic interface that allows both humans and computers to discover and understand service capabilities without needing access to the source code.

---

## 🛠️ Prerequisites

Before you begin, ensure you have the following installed and configured:

- [x] **Docker Engine** (for containerization)
- [x] **Python 3.8+** (if running natively)
- [x] **cURL** or **Postman** (for CLI/GUI API testing)
- [x] Basic knowledge of terminal environments and RESTful paradigms.

---

## 💻 Local Development Setup

We will use the `flask_cors` and `flask-swagger-ui` blueprints to expose our Python functions as documented APIs.

### 1. Clone the Repository
```bash
git clone [https://github.com/enterprise-devops/cloud-microservices.git](https://github.com/enterprise-devops/cloud-microservices.git)
cd cloud-microservices/swagger_infrastructure_api

```

### 2. Install Dependencies

Bash

```
python3 -m pip install flask flask_cors flask-swagger-ui

```

### 3. Run the Application

Start the Flask application, which serves the REST API on port `5000`.

Bash

```
python3 app.py

```

> **Note**: Your API is now running locally. You can access the raw JSON spec by navigating to `http://localhost:5000/swagger.json`.

----------

## 🐳 Docker Deployment: Cloud Status API

To simulate a real-world microservice, we will containerize a **Cloud Regional Status API** (replacing legacy tutorial examples).

### 1. Prepare the Microservice Controller

Navigate to your service controller (e.g., `controllers/regional_status_controller.py`) and implement the core logic:

Python

```
import connexion
import six
from swagger_server import util

def status_get():  # noqa: E501
    """
    Returns the real-time operational status of global cloud datacenters.
    :rtype: dict
    """
    datacenter_status = {
        "us-east-1": "🟢 Operational",
        "us-west-2": "🟢 Operational",
        "eu-central-1": "🟡 Degraded Performance",
        "ap-south-1": "🟢 Operational",
        "sa-east-1": "🔴 Outage Investigating",
        "ap-northeast-1": "🟢 Operational"
    }

    return datacenter_status

```

### 2. Build the Docker Image

Ensure you are in the directory containing the `Dockerfile` and execute:

Bash

```
docker build . -t cloud-status-api

```

### 3. Run the Container

Map port `8080` of the container to port `8080` of your host machine.

Bash

```
docker run --rm -p 8080:8080 cloud-status-api

```

### 4. Verify Container Health

Check if the container is running and port mapping is correct (`0.0.0.0:8080->8080/tcp`):

Bash

```
docker ps -a

```

> **Troubleshooting**: If you need to stop the container, find the container ID via `docker ps | grep cloud-status-api` and run `docker kill <container_id>`.

----------

## 📖 API Documentation (Swagger UI)

Swagger automatically builds an engaging UI and interactive API documentation by reading your `swagger_config.json` or YAML file.

### Viewing the Docs via Swagger Editor

1.  Open the [Swagger Editor](https://editor.swagger.io/).
    
2.  From the File menu, click **Clear Editor**.
    
3.  Paste the contents of your local `swagger_config.json` into the left pane.
    
4.  _(Optional)_ If prompted to convert JSON to YAML, you can click Cancel to retain the JSON format.
    
5.  The interactive UI will populate on the right.
    

### Testing the Endpoints

1.  Expand the `GET /status` endpoint.
    
2.  Click **Try it out**.
    
3.  Click **Execute**.
    
4.  View the HTTP `200 OK` response returning the application/json payload of datacenter statuses.
    

Alternatively, test directly from your terminal using cURL:

Bash

```
curl -X GET http://localhost:8080/status -H "accept: application/json"

```

----------

## 📚 Glossary

**Term**

**Definition**

**API Gateway**

API management tool that sits between a client and a collection of backend services.

**CRUD**

Creating, reading, updating, and deleting records.

**cURL**

A command-line tool and library for transferring data with URLs.

**DELETE**

HTTP request to delete a record.

**Flask**

A micro web framework for Python that does not require particular tools or libraries.

**GET**

HTTP request to retrieve a record.

**GraphQL**

A query language that enables you to retrieve exactly what you need from the API.

**OpenAPI Specification**

Defines a standard, language-agnostic interface to RESTful APIs.

**pip**

Python package manager.

**POST**

HTTP request to create a record.

**Postman**

An API platform for building, testing, and using APIs visually.

**PUT**

HTTP request to update a record.

**REST**

Representational State Transfer; an architectural style for distributed systems.

**Stateless**

Means each request contains all the information required to process it independently.

**Swagger**

Toolset that allows you to describe the structure of your APIs so that machines can read them, generating UI docs automatically.

---

# ☁️ Enterprise Deployment Patterns: Leveraging IBM Cloud Code Engine

> **DevOps Engineering Focus**: Transitioning from manual infrastructure orchestration to fully managed, serverless execution environments for production-grade microservices and batch processing.

---

## 🏗️ 1. The Challenge of Infrastructure Overhead
Self-hosting microservices introduces non-trivial operational complexity. For a production environment to be "Elite," an engineer must solve multiple infrastructure layers manually:

* **Artifact Orchestration**: Managing dependencies, credentials, and binary compilation.
* **Infrastructure Selection**: Provisioning OS, web servers, and database networking.
* **Elastic Scalability**: Implementing logic to handle fluctuating traffic (e.g., E-commerce holiday peaks).
* **Observability**: Setting up centralized logging, monitoring agents, and health-check dashboards.
* **Security & Isolation**: Ensuring inter-service communication is encrypted and workloads are isolated.

---

## 🚀 2. Solution: IBM Cloud Code Engine
IBM Cloud Code Engine (CE) is a **Fully Managed Serverless Platform** that abstracts the underlying cluster management. It unifies features from **PaaS, CaaS, and FaaS** into a single developer experience.

### 🛠️ Core Use Cases
| Mode | Description | Trigger |
| :--- | :--- | :--- |
| **Application Deployment** | Deploy pre-built container images (Microservices/Web Apps). | HTTPS Requests |
| **Source-to-Cloud** | Build directly from GitHub or local source code without Dockerfiles. | Code Push / CLI Build |
| **Batch Computing** | Execute heavy data processing or analytics tasks in isolation. | Manual / Scheduled |

---

## ⚡ 3. Python Runtime Standards: WSGI vs. ASGI
When deploying Python-based microservices (Flask, Django, FastAPI), selecting the correct interface is critical for performance.

### **WSGI (Web Server Gateway Interface)**
* **Standard**: Synchronous communication.
* **Production Stack**: Gunicorn, uWSGI.
* **Best For**: Traditional REST APIs with predictable blocking operations.

### **ASGI (Asynchronous Server Gateway Interface)**
* **Standard**: Supports Asynchronous code (async/await).
* **Production Stack**: Daphne, Hypercorn, Uvicorn.
* **Best For**: High-concurrency applications, WebSockets, and non-blocking I/O.

---

## 🔐 4. Configuration Management & Security Posture
A professional DevOps workflow dictates that **secrets must never be hardcoded**. 

### **Best Practices for Secrets**
1.  **Zero-Inclusion Policy**: Avoid placing passwords or API tokens in the source code.
2.  **Environment Variables**: Inject sensitive data as `env` variables during the deployment phase.
3.  **Scope Control**: Use ConfigMaps for non-sensitive settings and Secrets for encrypted data.

### **Automatically Injected Variables**
IBM Code Engine enjected specific metadata into runtime environments:
* `JOB_INDEX`: A unique identifier (starting at 0) for parallel job instances.
* `CE_DOMAIN` & `CE_SUBDOMAIN`: Used to programmatically resolve internal/external URLs for service-to-service communication.
    * *Example URL Format*: `appName.CE_SUBDOMAIN.CE_DOMAIN`

---

## ⌨️ 5. Deployment Orchestration (CLI vs. Console)

### **A. CLI Method (Recommended for CI/CD Pipelines)**
The CLI offers high flexibility and repeatability. Ensure the `code-engine` plugin is installed.
```bash
# Example: Creating an app with environment variables
ibmcloud ce app create --name prod-api --image us.icr.io/namespace/image:v1 --env API_KEY=secret_token

```

### **B. Console Method (Recommended for Discovery)**

The IBM Cloud Portal provides an intuitive UI to:

-   Define literal environment variables.
    
-   Reference existing **Secrets** or **ConfigMaps**.
    
-   Update or delete configurations without redeploying the entire binary.
    

----------

## 🛡️ 6. Enterprise-Grade Benefits

-   **Rapid Deployment**: Move from source to live HTTPS endpoint in seconds.
    
-   **Native IAM Integration**: Access control is governed by IBM Cloud Identity and Access Management.
    
-   **Automatic TLS**: Transport Layer Security is managed at the platform level.
    
-   **Zero-Scale**: Automatically scale down to zero when idle to eliminate unnecessary costs.

---




# ☸️ IBM Cloud Code Engine: Enterprise Resource Orchestration

> **Engineering Brief**: A technical deep-dive into **IBM Cloud Code Engine (CE)**—a high-density, serverless platform designed to unify Container-as-a-Service (CaaS) and Platform-as-a-Service (PaaS) capabilities into a single, cohesive developer experience.

![Platform](https://img.shields.io/badge/IBM_Cloud-Code_Engine-052FAD.svg)
![Runtime](https://img.shields.io/badge/Runtime-Serverless-lightgrey.svg)
![Orchestration](https://img.shields.io/badge/Orchestration-Kubernetes_Based-326CE5.svg)
![Security](https://img.shields.io/badge/Security-TLS_Integrated-success.svg)

---

## 🏛️ 1. Project Hierarchy & Isolation
In a professional DevOps environment, resource isolation is paramount. Code Engine uses **Projects** as the top-level grouping mechanism.

* **Namespacing**: Projects provide logical isolation. Entity names (apps, builds, jobs) must be unique within a project but can overlap across different projects.
* **Access Control & Auditing**: Integrated with IBM IAM to define who can modify or view resources.
* **Resource Quotas**: Hard limits on Total CPU and Memory allocation are managed at the Project level to prevent runaway costs or resource exhaustion.

---

## 🚀 2. Core Operational Entities

| Entity | Primary Function | Protocol / Lifecycle |
| :--- | :--- | :--- |
| **Application** | Serves long-running workloads. | HTTPS / WebSockets (TCP-based) |
| **Job** | One-time execution for batch processing. | Run-to-Completion / Exit |
| **Build** | Orchestrates Source-to-Image conversion. | Dockerfile / Cloud Native Buildpacks |

### 📈 Scaling Dynamics
Code Engine utilizes **Automatic Horizontal Scaling**. Instances are created or destroyed based on incoming concurrency.
* **Scale-to-Zero**: When no traffic is detected, instances are terminated to eliminate costs.
* **Scale-up**: Rapid instantiation of new containers to handle traffic spikes.



---

## 🐳 3. Containerization Strategy: Docker Fundamentals

A **Container Image** is a read-only template (comparable to a **Class** in OOP), while a **Container** is the running instance (the **Object**).



### The Blueprint (Dockerfile)
A professional Dockerfile follows a deterministic sequence:
1.  `FROM`: Inherit from a trusted base image (e.g., `python:3.9-slim`).
2.  `COPY`: Transfer application artifacts.
3.  `RUN`: Execute build-time commands (e.g., `pip install`).
4.  `ENV`: Set runtime configuration variables.
5.  `EXPOSE`: Document the intended listening port (e.g., `8080`).
6.  `CMD`: Define the entrypoint execution command.

---

## 🔌 4. Service Bindings & Integration
Harnessing the power of the IBM Cloud ecosystem requires secure, automated connectivity.

* **Service Bindings**: A bridge mechanism that links a Code Engine entity to an IBM Cloud service (e.g., Cloudant, Object Storage).
* **Automated Credential Injection**: Upon binding, CE automatically generates a **Service Access Secret** and injects these credentials into the container’s environment variables as a JSON object.
* **IAM Alignment**: All bindings respect Identity and Access Management policies, ensuring that only authorized services can communicate.

---

## ⌨️ 5. Hands-on Operations (CLI Guide)

Ensure you are logged into the `ibmcloud` CLI and have selected the correct resource group.

### Deploying from Source (Git-to-App)
This command triggers an internal build process, pushes the image to a private registry, and deploys the application.
```bash
ibmcloud ce application create \
  --name production-api \
  --build-source [https://github.com/org/repo-name](https://github.com/org/repo-name) \
  --image us.icr.io/${NAMESPACE}/app-image:latest \
  --registry-secret icr-secret \
  --port 8080

```

### Resource Management

Bash

```
# List all active applications in the project
ibmcloud ce app list

# Inspect detailed metadata, instances, and usage
ibmcloud ce app get --name production-api

# Execute a one-time data processing job
ibmcloud ce jobrun submit --name batch-processor-run --job batch-processor
```

---



# 🚀 Enterprise Operations: Lifecycle Management & Microservices Scaling

> **Operational Objective**: Mastery of zero-downtime updates, horizontal scaling, and service visibility within IBM Cloud Code Engine. This guide focuses on the technical orchestration of production-grade microservices.

---

## 🔄 1. Revision Management & Blue-Green Patterns
IBM Cloud Code Engine treats infrastructure as immutable. When an application is updated, the platform does not overwrite the existing instance; it creates a new **Revision**.

* **Traffic Shifting**: Code Engine automatically routes traffic to the latest successful revision.
* **Rollback Capability**: Since previous revisions are preserved, engineers can quickly revert to a known-good state if a new deployment fails health checks.
* **Zero-Downtime**: New instances are spun up and verified before the old revision is decommissioned.



---

## 🛠️ 2. Critical Update Scenarios
In a DevOps lifecycle, updates generally fall into four technical categories:

| Update Category | Scope | CLI Flag / Attribute |
| :--- | :--- | :--- |
| **Environment** | Database credentials, API endpoints, Secret keys. | `--env` / `--env-from` |
| **Visibility** | Shifting service access from Public to Private/Internal. | `--visibility` |
| **Registry/Image** | Deploying a new container build or shifting Git branches. | `--image` / `--build-source` |
| **Resource Profile** | Adjusting CPU/Memory to handle compute-intensive loads. | `--cpu` / `--memory` |

---

## 🌐 3. Network Visibility & Domain Mappings
Security posture is defined by how services are exposed. Code Engine provides granular control over the "Service Perimeter."

* **Cluster-Local (Internal)**: Application is only reachable by other services within the same Project. Ideal for back-end databases or internal processing logic.
* **IBM Private Network**: Accessible via Virtual Private Endpoints (VPE), keeping traffic within the IBM backbone.
* **Public**: Accessible via the open internet with automatically managed TLS certificates.

> **Command Ref**: `ibmcloud ce app update --name <app-name> --visibility private`

---

## 📈 4. Horizontal Scaling & Performance Tuning
Unlike monolithic architectures, microservices allow for independent scaling based on specific bottleneck metrics.

### **Concurrency & Scaling Parameters**
* **Min Instances (`--min`)**: Ensures a baseline of "Warm" instances to eliminate cold-start latency.
* **Max Instances (`--max`)**: Prevents "Denial of Wallet" by capping the maximum horizontal expansion.
* **Concurrency**: Defines how many simultaneous requests a single instance handles before the platform triggers a scale-out event.



---

## 🏗️ 5. Case Study: UK University Microservices Architecture
In this implementation, the system is decoupled into two distinct services to allow for independent lifecycle management.

### **Architecture Overview**
1.  **Listing Service**: Handles high-volume keyword searches for university names.
2.  **Websites Service**: A specialized service for retrieving domain-specific metadata.

### **Operational Workflow**
```bash
# 1. Scaling the Listing Service to handle high traffic
# We set a floor of 2 instances to ensure immediate responsiveness.
ibmcloud ce app update --name listing --min 2

# 2. Hot-Patching the Website Service
# Updating logic to support keyword search without redeploying the entire project.
ibmcloud ce app update --name websites \
  --build-source [https://github.com/org/repo](https://github.com/org/repo) \
  --build-context-dir websites

```

----------

## 💻 6. DevOps CLI Cheat Sheet

### **Application Resource Modification**

Bash

```
# Increase compute footprint for heavy NoSQL operations
ibmcloud ce app update --name pet-db-service --cpu 2 --memory 16G

```

### **Secret & Configuration Injection**

Bash

```
# Injecting new database host without code changes
ibmcloud ce app update --name pet-db-service --env DB_HOST=nosql.internal.com

```

### **Audit & Inspection**

Bash

```
# Retrieve detailed state, including URL endpoints and instance health
ibmcloud ce app get --name listing

```

---




# 🏢 Enterprise Architecture: Polyglot Product Comparison System

> **Engineering Brief**: Deployment documentation for the distributed **Dealer Evaluation & Product Pricing System**. This project leverages a polyglot microservices architecture (Python & Node.js) deployed via Source-to-Image (S2I) pipelines on IBM Cloud Code Engine.

![Architecture](https://img.shields.io/badge/Architecture-Microservices-purple.svg)
![Platform](https://img.shields.io/badge/IBM_Cloud-Code_Engine-052FAD.svg)
![Backend1](https://img.shields.io/badge/Service-Python_REST_API-3776AB.svg)
![Backend2](https://img.shields.io/badge/Service-Node.js_REST_API-339933.svg)

---

## 🏗️ 1. System Architecture & Monorepo Strategy

The application is decoupled into three distinct serverless components. The backends are stored in a Monorepo structure, requiring specific build-context directories during the containerization phase.

| Service Entity | Runtime | Target Port | Build Context Dir | Primary Function |
| :--- | :--- | :--- | :--- | :--- |
| **`prodlist`** | Python | `5000` | `/products_list` | Product catalog & metadata retrieval |
| **`dealerdetails`** | Node.js | `8080` | `/dealer_details` | Real-time dealer pricing engine |
| **`frontend`** | HTML/JS | `5001` | `/ (Root)` | Client-side UI & Service Aggregation |

---

## 🚀 2. Infrastructure Provisioning (Backend)

Ensure your IBM Cloud CLI is authenticated and your Code Engine project is targeted. We will deploy the backend services directly from the Git repository. 

### Phase 2A: Product Details Service (Python)
This service exposes the product catalog.

```bash
# Deploy Product Catalog Service
ibmcloud ce application create \
  --name prodlist \
  --image us.icr.io/${SN_ICR_NAMESPACE}/prodlist \
  --registry-secret icr-secret \
  --port 5000 \
  --build-context-dir products_list \
  --build-source [https://github.com/ibm-developer-skills-network/dealer_evaluation_backend.git](https://github.com/ibm-developer-skills-network/dealer_evaluation_backend.git)

# Verify Deployment and extract the external URL
ibmcloud ce app get --name prodlist --output json | grep url

```

### Phase 2B: Dealer Pricing Service (Node.js)

This service exposes the dynamic pricing logic based on dealer inventory.

Bash

```
# Deploy Dealer Pricing Service
ibmcloud ce application create \
  --name dealerdetails \
  --image us.icr.io/${SN_ICR_NAMESPACE}/dealerdetails \
  --registry-secret icr-secret \
  --port 8080 \
  --build-context-dir dealer_details \
  --build-source [https://github.com/ibm-developer-skills-network/dealer_evaluation_backend.git](https://github.com/ibm-developer-skills-network/dealer_evaluation_backend.git)

# Verify Deployment and extract the external URL
ibmcloud ce app get --name dealerdetails --output json | grep url

```

> **⚠️ DevOps Note - Revision Collisions:** If you encounter a `FAILED Wait failed for application` error, it indicates a dirty namespace or hanging revision from a previous run. Append an iteration suffix (e.g., `--name prodlist-v2`) or delete the existing orphaned app before redeploying.

----------

## 🔗 3. Service Discovery & Frontend Integration

The frontend operates as a client-side Single Page Application (SPA). Because it renders in the user's browser, it must be injected with the public external URLs of the backend services prior to deployment.

### 3A: Environment Configuration

Clone the frontend repository into your build environment:

Bash

```
cd /home/project
git clone [https://github.com/ibm-developer-skills-network/dealer_evaluation_frontend.git](https://github.com/ibm-developer-skills-network/dealer_evaluation_frontend.git)
cd dealer_evaluation_frontend

```

**Manual Configuration Step:** Open `index.html` and locate the API routing variables. Replace the `localhost` development placeholders with the active Code Engine URLs retrieved in Step 2.

JavaScript

```
// Example modification inside index.html
const PRODUCT_API_URL = "[https://prodlist.xxxxxx.us-south.codeengine.appdomain.cloud/](https://prodlist.xxxxxx.us-south.codeengine.appdomain.cloud/)";
const DEALER_API_URL = "[https://dealerdetails.xxxxxx.us-south.codeengine.appdomain.cloud/](https://dealerdetails.xxxxxx.us-south.codeengine.appdomain.cloud/)";

```

_Requirement: Ensure both URLs end with a trailing slash (`/`)._

### 3B: Frontend Deployment

Once the routing endpoints are hardcoded into the asset, deploy the frontend application from your local workspace (`.`).

Bash

```
# Deploy the Frontend Aggregator
ibmcloud ce application create \
  --name frontend \
  --image us.icr.io/${SN_ICR_NAMESPACE}/frontend \
  --port 5001 \
  --build-source .

```

----------

## 🧪 4. Operational Validation & Testing

Once the frontend deployment returns a successful public URL, access the application via a web browser and validate the following cross-service communication pathways:

1.  **Catalog Retrieval**: Click the `Products` dropdown. This triggers an asynchronous `GET` request to the `prodlist` Python service. Verify the UI populates with valid inventory.
    
2.  **Point-to-Point Pricing**: Select a specific Product, then select a specific Dealer. This triggers a request to the `dealerdetails` Node.js service. Verify the exact price renders.
    
3.  **Aggregation Logic**: Select a Product with multiple dealers, and choose the `All Dealers` option. Verify the system successfully maps and iterates through the JSON payload, displaying a comparative pricing grid.
