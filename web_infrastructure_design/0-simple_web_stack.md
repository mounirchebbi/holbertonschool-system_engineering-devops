# One Server Web Infrastructure for www.foobar.com

## Overview

This design illustrates a simple web infrastructure using one server that hosts the website accessible at `www.foobar.com`. It uses a LAMP-style stack, replacing Apache with **Nginx**, and includes:

- 1 domain name: `foobar.com` with `www` subdomain
- 1 server at IP address `8.8.8.8`
  - Web server: Nginx
  - Application server: Handles code execution (e.g., PHP or Python)
  - Application codebase
  - Database: MySQL

---

## Step-by-Step Flow

1. A **user** opens their browser and types `www.foobar.com`.
2. The DNS resolves `www.foobar.com` to the IP `8.8.8.8`.
3. The user's browser sends an HTTP(S) request to `8.8.8.8`.
4. The **Nginx web server** receives the request.
5. Nginx forwards the request to the **application server**, which executes the code (e.g., PHP scripts).
6. The application server may query the **MySQL database** for dynamic content.
7. The application server sends a response back to Nginx.
8. Nginx sends the final HTTP(S) response to the user's browser.

---

## Infrastructure Diagram (Mermaid)

```mermaid
graph TD
    A[User] -->|HTTP Request| B[DNS - Resolves www.foobar.com to 8.8.8.8]
    B --> C[Single Server: 8.8.8.8]
    C --> D[Nginx Web Server]
    D --> E[Application Server]
    E --> F[Application Codebase]
    E --> G[MySQL Database]
    E -->|Response| D
    D -->|HTTP Response| A
