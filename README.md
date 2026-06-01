#  Secure Multi-Tier Infrastructure with Nginx, Tomcat, MariaDB and Keycloak

##  Overview

This project demonstrates the implementation of a secure multi-tier infrastructure designed to host multiple web applications using centralized authentication, encrypted communications and logical service segregation.

The environment was developed as part of a Software Engineering project and focuses on infrastructure, security and authentication concepts commonly found in enterprise environments.

### Hosted Applications

*  EstoqueManager
*  PrefPet

---

#  Architecture

<img width="1999" height="532" alt="Image" src="https://github.com/user-attachments/assets/8f503571-46df-4f65-b077-d6c188281dc4" />

### Infrastructure Layout

```text
Internet
    │
    ▼
Nginx Reverse Proxy (VM1)
    │
    ▼
Tomcat + Keycloak (VM2)
    │
    ▼
MariaDB Database Server (VM3)
```

### VM1 - Frontend Layer

Responsible for:

* Hosting Angular applications
* Reverse Proxy
* HTTPS Redirection
* TLS Termination
* Routing Requests

**Technologies**

* Alpine Linux
* Nginx
* TLS Certificates

---

### VM2 - Application Layer

Responsible for:

* Hosting Java Applications
* Authentication Services
* Business Logic

**Technologies**

* Apache Tomcat
* Spring Boot
* Java
* Keycloak

Hosted Applications:

```text
ROOT.war              -> EstoqueManager
backendprefpet.war    -> PrefPet
```

---

### VM3 - Database Layer

Responsible for:

* Persistent Storage
* Database Isolation
* Secure Connections

**Technologies**

* MariaDB

Dedicated Database Users:

```text
estoquemanager
prefpet
keycloak
backend
```

---

#  Authentication Architecture

Authentication is centralized using Keycloak.

## Authentication Flow

```text
User
 │
 ▼
Frontend Application
 │
 ▼
Keycloak Login
 │
 ▼
JWT Token Issued
 │
 ▼
Backend Validation
 │
 ▼
Authorized Access
```

### Keycloak Features

* Centralized Authentication
* Single Sign-On (SSO)
* JWT Token Generation
* Role-Based Access Control (RBAC)

### Clients

```text
estoquemanager
prefpet
```

### Example Roles

```text
ADMIN
VENDEDOR
TUTOR
```

---

#  Security Features

## Reverse Proxy

Implemented using Nginx to isolate backend services from direct internet exposure.

Example:

```nginx
location /api/ {
    proxy_pass https://back.local.projetomensal.com.br:8443;
}

location /keycloak/ {
    proxy_pass http://back.local.projetomensal.com.br:8081;
}
```

---

## TLS Encryption

TLS is implemented across all critical layers.

### Protected Communications

* Browser ↔ Nginx
* Nginx ↔ Tomcat
* Application ↔ Database

### Certificate Information

```text
Issuer:
Projetomensal Intermediate CA

Subject:
*.local.projetomensal.com.br

Algorithm:
RSA 2048

Validity:
2026 - 2027
```

Certificate validation:

```bash
openssl x509 -in fullchain.pem -text -noout
```

---

#  Database Security

MariaDB communication is encrypted using TLS.

Validation:

```sql
SHOW STATUS LIKE 'Ssl_cipher';
```

Result:

```text
TLS_AES_256_GCM_SHA384
```

This confirms encrypted communication between application servers and the database.

### Database User Segregation

```text
backend
estoquemanager
prefpet
keycloak
```

Each application uses a dedicated database user to reduce privilege exposure.

---

#  Reverse Proxy Configuration

Example Routing:

```text
https://front.local.projetomensal.com.br
                │
                ▼
             Nginx
                │
                ▼
https://back.local.projetomensal.com.br:8443
```

Authentication Proxy:

```text
https://front.local.prefpet.com.br/keycloak
                │
                ▼
             Keycloak
```

---

# ⚙️ Tomcat HTTPS Configuration

Tomcat was configured to accept HTTPS connections.

Example:

```xml
<Connector
    port="8443"
    protocol="org.apache.coyote.http11.Http11NioProtocol"
    SSLEnabled="true"
/>
```

Validation:

```bash
grep -n "Connector" /opt/tomcat/conf/server.xml
```

---

# 🛠️ Technologies

## Infrastructure

* Alpine Linux
* Nginx
* Apache Tomcat
* MariaDB
* Docker

## Security

* TLS 1.2
* TLS 1.3
* JWT
* Keycloak
* Reverse Proxy

## Applications

* Angular
* Java
* Spring Boot

---

#  Results

The project successfully achieved:

 Secure Multi-Tier Architecture

 Centralized Authentication

 JWT Authorization

 TLS-Protected Communications

 Reverse Proxy Implementation

 Database Isolation

 Multi-Application Hosting

 Enterprise-Style Infrastructure Deployment

---

#  Future Improvements

* Prometheus Monitoring
* Grafana Dashboards
* High Availability
* CI/CD Pipelines
* Container Orchestration
* Automated Deployments

---

#  Lessons Learned

This project provided hands-on experience with:

* Linux Server Administration
* Reverse Proxy Configuration
* TLS Certificate Management
* Database Security
* Authentication and Authorization
* Enterprise Infrastructure Design
* Application Deployment

---

#  Author

**Kristhian dos Santos**

Software Engineering Student

Focused on:

* Infrastructure
* Systems Administration
* Security
* Backend Development
