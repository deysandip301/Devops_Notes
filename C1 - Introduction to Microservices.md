# 🏛️ 1. Core Concept: Monolith vs. Microservices

* **Monolithic:** A single, unified codebase containing all application services and logic.
* **Microservices:** The application is broken down into smaller, independent codebases (services) built around specific business capabilities (e.g., payments, orders) that communicate via APIs.

---

# 🧩 2. Core Architectural Components

| Component                        | Description                                                                                                          |
| :------------------------------- | :------------------------------------------------------------------------------------------------------------------- |
| **API Gateway**                  | Acts as the single entry point for all client requests. Handles routing, authentication, rate limiting, and caching. |
| **Service Registry / Discovery** | Keeps track of all active, healthy service instances and their locations.                                            |
| **DNS**                          | Maps human-readable service names (domain names) to IP addresses.                                                    |
| **Load Balancer**                | Distributes incoming traffic across multiple instances of a service to ensure high availability and scalability.     |
| **Database per Service**         | Each microservice owns its own database to maintain loose coupling.                                                  |
| **Synchronous Comm.**            | Request-response communication where the caller waits (e.g., HTTP/REST, gRPC).                                       |
| **Asynchronous Comm.**           | Event-driven communication via message brokers where the caller does not wait.                                       |
| **Message Broker**               | Handles async communication, message queues, and event streaming (e.g., Kafka, RabbitMQ).                            |
| **Externalized Config**          | Configuration is centralized and stored (e.g., in environment variables), not in the code.                           |
| **Externalized Logs**            | A central logging service (like ELK, Splunk) that collects logs from all services.                                   |
| **Monitoring / Tracing**         | Observability tools (e.g., Prometheus, Grafana, Jaeger) to track service health, performance, and errors.            |

---

# 🌊 3. The Total Flow: A Step-by-Step Request

1.  **Browser** queries **DNS** to translate a domain name (e.g., `myapp.com`) to an IP address.
2.  **DNS Resolver** queries the server chain (Root $\rightarrow$ TLD $\rightarrow$ Authoritative) and returns the IP.
3.  **Browser** sends its request to that IP, hitting the main **Load Balancer**.
4.  **Load Balancer** distributes traffic to the single entry point: the **API Gateway**.
5.  **API Gateway** handles **Authentication/Authorization** (e.g., checking tokens).
6.  The Gateway uses **Service Discovery** to find the current network location of the correct microservice.
7.  The Gateway routes the request (e.g., using path-based routing) to the specific service instance.
8.  That service may use **Synchronous** (blocking) communication to call another service.
9.  It may also use **Asynchronous** (non-blocking) communication by publishing an event to a **Message Broker**.
10. Other services (e.g., "Notification," "Analytics") consume this event using a **Pub-Sub** or **Queue Model**.
11. Each service interacts with its **own private Database** to read or write data.
12. Throughout this flow, all services stream **Logs** to a **central, externalized logging service**.

---

# ⚙️ 4. Key Components (In-Depth)

#### DNS (Domain Name System)
* **Purpose:** DNS translates human-readable domain names (like `myapp.com`) into IP addresses (like `192.168.1.2`).
* **How It Works:**
    1.  Client requests `myapp.com`.
    2.  DNS resolver queries: Root Server $\rightarrow$ TLD Server (.com) $\rightarrow$ Authoritative Server.
    3.  Returns IP address (e.g., `54.12.32.18`).
    4.  Browser connects to that IP.

#### Load Balancers
* **Purpose:** A load balancer (LB) evenly distributes incoming requests among multiple backend servers to ensure:
    * High availability
    * Fault tolerance
    * Scalability
* **Types of Load Balancers:**

| Type | Operates On | Example | Use Case |
| :--- | :--- | :--- | :--- |
| L4 | TCP/UDP (Transport) | AWS NLB, HAProxy | Fast, for network-level routing |
| L7 | HTTP/HTTPS (Application) | AWS ALB, Nginx, Traefik | Smarter, inspects URLs, headers, cookies |
   
* **Features:** Health checks, Sticky sessions, SSL termination, Auto-scaling integration.

#### API Gateway
* **Role:** An API Gateway sits between the client and backend services, acting as the single entry point.
* **Responsibilities:**

| Feature | Description |
| :--- | :--- |
| Routing | Direct requests to proper microservice. |
| Authentication / Authorization | Validate tokens (OAuth2, JWT) |
| Rate Limiting & Throttling | Protect from overload. |
| Request Transformation | Modify headers, URLs, or payloads. |
| Caching | Improve response speed. |
| Monitoring / Logging | Centralized request tracing. |

#### Externalizing Logs
* **Why Externalize?** Each microservice runs in containers/pods; their **local logs vanish when the container restarts**.
* **Solution:** Logs are centralized in an external system.
* **Common Setup:**
    * **Collect logs:** Fluentd / Fluent Bit / Logstash.
    * **Store & index logs:** Elasticsearch / Loki.
    * **Visualize & search logs:** Kibana / Grafana.
    * **Other Tools:** CloudWatch (AWS), Stackdriver (GCP), Splunk, Dynatrace.

---

# 🗣️ 5. Communication & Data Patterns

#### Synchronous vs. Asynchronous Communication

| Aspect | Synchronous | Asynchronous |
| :--- | :--- | :--- |
| **Definition** | Caller waits for response. | Caller sends message and continues. |
| **Example** | REST API call. | Message queue (Kafka, RabbitMQ). |
| **Protocol** | HTTP/HTTPS. | AMQP, MQTT, Kafka. |
| **Use Case** | Real-time responses (e.g., login). | Decoupled processing (e.g., notifications, billing). |
| **Drawback** | Tight coupling, latency-sensitive. | Complex to debug, eventual consistency. |

#### SQL vs. NoSQL Databases

| Feature | SQL (Relational) | NoSQL (Non-relational) |
| :--- | :--- | :--- |
| **Structure** | Tables, rows, columns. | Key-value, Document, Graph, Wide-column. |
| **Schema** | Fixed. | Dynamic. |
| **Scaling** | **Vertical** (add more CPU/RAM). | **Horizontal** (add more nodes). |
| **Examples** | MySQL, PostgreSQL. | MongoDB, DynamoDB, Cassandra. |
| **Use Cases** | Transactions, structured data. | High scalability, flexible schema. |
| **Query Language** | SQL. | Custom (JSON-like). |

> **Polyglot Persistence:** Microservices often use *both* types, picking the best database for each service's specific needs. The architecture itself is "polyglot" (language-independent).

---

# 📜 6. 12-Factor App Philosophy

A design philosophy for building scalable, cloud-ready microservices.

1.  **Codebase:** One codebase in version control (e.g., one GitHub repo per service), many deploys.
2.  **Dependencies:** Explicitly declare and isolate dependencies.
3.  **Config:** Store configuration (like passwords) in **environment variables**, *not* in the code.
4.  **Backing Services:** Treat databases, queues, etc., as attached resources.
5.  **Build, Release, Run:** Strictly separate these stages.
6.  **Processes:** Execute the app as one or more **stateless processes**.
7.  **Port Binding:** Expose services via port binding.
8.  **Concurrency:** Scale out via the process model (e.g., adding more containers).
9.  **Disposability:** Favor fast startup and graceful shutdown.
10. **Dev/Prod Parity:** Keep environments as similar as possible.
11. **Logs:** Treat logs as **event streams**.
12. **Admin Processes:** Run admin/management tasks as one-off processes.

> **Mnemonic:** A good trick to remember these is "**C**ool **D**evelopers **C**an **B**uild **B**etter **P**ortable **C**loud **D**ata-Driven **P**roducts **L**og **A**dmin" (or the letter sequence: C, D, C, B, B, P, P, C, D, D, L, A).