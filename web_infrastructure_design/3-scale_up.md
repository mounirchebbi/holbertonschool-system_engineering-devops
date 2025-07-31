# Scaled-Up Secure and Monitored Web Infrastructure for www.foobar.com

## Purpose of Scaling

As traffic to `www.foobar.com` grows, scaling the infrastructure ensures high availability, better performance, and isolation of responsibilities across components. This improves **fault tolerance**, **load distribution**, and **security**.

---

## New Components and Their Purpose

### 1. Redundant Load Balancers
- Two **HAProxy load balancers** are configured in a **cluster** for failover.
- A **firewall layer** protects this entry point.
- **Purpose:** Ensures that if one load balancer fails, the other continues to serve traffic.

### 2. Isolated Web Servers
- Dedicated **Nginx web servers** now handle only HTTP(S) routing.
- **Purpose:** Reduces load on application logic servers and improves modularity.

### 3. Separated Application Servers
- Two **application servers** are used to run application logic.
- **Purpose:** Allows scaling independently of web or DB layers, and balances backend load.

### 4. Dedicated Database Servers
- One **primary MySQL server** for write operations.
- One **replica MySQL server** for read operations.
- **Purpose:** Improves DB performance and reliability via read/write splitting.

### 5. Enhanced Monitoring
- Each app server sends logs and metrics to a central **monitoring service**.
- **Purpose:** Enables observability and proactive incident detection.

---

## Step-by-Step Flow

1. User sends a request to `https://www.foobar.com`.
2. DNS resolves the domain.
3. Request passes through a **firewall** protecting the load balancers.
4. **HAProxy load balancer 1 or 2** handles SSL termination and selects a web server.
5. Request passes through a **web-tier firewall** to a selected **Nginx web server**.
6. Nginx forwards the request to an **application server**.
7. The application server processes it using business logic.
8. Depending on the query, it communicates with either:
   - **Primary DB** for writes
   - **Replica DB** for reads
9. Logs and metrics from application servers are collected by monitoring clients.
10. Monitoring data is sent to a centralized service like **Sumo Logic**.

---

## Infrastructure Diagram (Mermaid)

```mermaid
graph TD
    A[User] -->|HTTPS| B[DNS: www.foobar.com]

    B --> C1[Firewall - LB Layer]
    C1 --> D1[HAProxy LB 1]
    C1 --> D2[HAProxy LB 2]
    D1 -->|Decrypted HTTP| FW1[Firewall - Web Tier]
    D2 -->|Decrypted HTTP| FW1

    FW1 --> W1[Nginx Web Server 1]
    FW1 --> W2[Nginx Web Server 2]

    W1 --> APP1[App Server 1]
    W2 --> APP2[App Server 2]
    APP1 --> CODE1[App Logic 1]
    APP2 --> CODE2[App Logic 2]

    CODE1 --> DB1[MySQL Primary DB]
    CODE2 --> DB2[MySQL Replica DB]

    APP1 --> M1[Monitoring Client 1]
    APP2 --> M2[Monitoring Client 2]
    M1 --> MS[Monitoring Service - Sumo Logic]
    M2 --> MS
