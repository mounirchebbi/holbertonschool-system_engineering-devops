# Secure and Monitored Web Infrastructure for www.foobar.com

## Overview

This infrastructure enhances availability, security, and observability of the website hosted at `www.foobar.com`. It includes:

- 1 **load balancer** with **SSL termination** (HAProxy)
- 2 **application servers**
- 3 **firewalls**
- **HTTPS** traffic with SSL certificate
- **Monitoring clients** to collect metrics and logs
- One **Primary-Replica** MySQL setup

---

## Step-by-Step Flow

1. The user types `https://www.foobar.com` in the browser.
2. DNS resolves the domain to the IP of the **HAProxy load balancer**.
3. The user's HTTPS request reaches **HAProxy**, where **SSL is terminated**.
4. HAProxy forwards the decrypted HTTP request to one of the backend servers.
5. The backend server processes the request using:
   - **Nginx (web server)**
   - **Application code**
   - **MySQL** (write goes to primary, read can go to replica)
6. Logs, metrics, and security events are collected and sent to a **monitoring service** (e.g., Sumo Logic).
7. Responses are returned through the same path, re-encrypted (if necessary), and served to the user.

---

## Infrastructure Diagram (Mermaid)

```mermaid
graph TD
    A[User] -->|HTTPS Request| B[DNS: www.foobar.com]
    B --> C[Firewall 1 (Load Balancer Layer)]
    C --> D[HAProxy Load Balancer (SSL Termination)]

    D -->|Decrypted HTTP| E[Firewall 2 (App Server 1)]
    D -->|Decrypted HTTP| F[Firewall 3 (App Server 2)]

    E --> E1[Nginx Web Server]
    E1 --> E2[App Server]
    E2 --> E3[App Code]
    E2 --> E4[MySQL Primary DB]

    F --> F1[Nginx Web Server]
    F1 --> F2[App Server]
    F2 --> F3[App Code]
    F2 --> F4[MySQL Replica DB]

    E & F --> G[Monitoring Clients]
    G --> H[Monitoring Service (e.g., Sumo Logic)]
