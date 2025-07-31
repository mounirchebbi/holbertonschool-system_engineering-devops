# Three-Server Web Infrastructure for www.foobar.com

## Overview

This infrastructure design improves upon a single-server setup by introducing **load balancing** and **redundancy**. It hosts the website at `www.foobar.com` using:

- 1 **load balancer**: HAProxy
- 2 **application servers**, each with:
  - Web server: Nginx
  - Application server (e.g., PHP-FPM)
  - Application codebase
  - MySQL database

---

## Step-by-Step Flow

1. The **user** types `www.foobar.com` into their browser.
2. The DNS resolves the domain to the IP address of the **HAProxy load balancer**.
3. HAProxy receives the request and forwards it to one of the **two backend servers**.
4. Each backend server runs:
   - **Nginx** to receive the request
   - An **application server** to process the logic
   - Optionally reads/writes from a **MySQL database**
5. The web server returns the response to HAProxy, which sends it to the user's browser.

---

## Infrastructure Diagram (Mermaid)

```mermaid
graph TD
    A[User] -->|HTTP Request| B[DNS: www.foobar.com -> LB IP]
    B --> C[HAProxy Load Balancer]

    C --> D[Web/App Server 1]
    C --> E[Web/App Server 2]

    D --> D1[Nginx]
    D1 --> D2[App Server]
    D2 --> D3[App Code]
    D2 --> D4[MySQL Primary DB]

    E --> E1[Nginx]
    E1 --> E2[App Server]
    E2 --> E3[App Code]
    E2 --> E4[MySQL Replica DB]
