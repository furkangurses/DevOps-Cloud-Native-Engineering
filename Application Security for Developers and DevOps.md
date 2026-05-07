# Network Security & Application Defense Architecture

This repository contains comprehensive documentation and architectural guidelines for implementing the **OSI Model**, **Multi-layered Security**, and **Modern Encryption Protocols** within a professional DevOps and Software Engineering context. 

---

## 1. The OSI Model: A Developer’s Perspective
Understanding the Open Systems Interconnection (OSI) model is critical for troubleshooting network bottlenecks and designing scalable cloud architectures. While the model defines seven layers, high-level engineers primarily interface with the top three layers for application logic and the middle layers for routing and reliability.

### The 7-Layer Stack
| Layer | Name | Data Unit | Responsibility |
| :--- | :--- | :--- | :--- |
| **L7** | **Application** | Data | Network services (HTTP/HTTPS, FTP, SMTP). |
| **L6** | **Presentation** | Data | Serialization, Encryption, Compression (JPEG, ASCII). |
| **L5** | **Session** | Data | Inter-host communication and session management. |
| **L4** | **Transport** | Segments | End-to-end connections, Reliability (TCP/UDP). |
| **L3** | **Network** | Packets | Path determination and IP (Routing). |
| **L2** | **Data Link** | Frames | Physical addressing (MAC), Error correction. |
| **L1** | **Physical** | Bits | Bitstream transmission over physical medium. |

> **Pro-Tip:** As a developer, focus on **L5 to L7**. Use **Port 443 (HTTPS)** at the Application layer to ensure encrypted traffic and build user trust through secure-by-default communication.

---

## 2. Multi-Layered Security (Defense in Depth)
A robust security posture requires a multi-layered approach to protect assets from the front end to the underlying cloud infrastructure.

### A. Web Application Security
*   **Frontend:** Sanitize inputs and enforce HTTPS across all devices.
*   **API/Middleware:** Secure the bridge between frontend and backend (Python, Java, Ruby) using strict authentication.
*   **Backend:** Implement least-privilege access for databases.

### B. Cloud & Infrastructure Defense
*   **IAM (Identity & Access Management):** Configure granular roles and permissions to secure cloud assets.
*   **Security Groups:** Implement firewall rules at the infrastructure level to restrict traffic to specific cloud resources.
*   **2FA/MFA:** Enforce multi-factor authentication for all administrative and user access points.

### C. Pipeline & Communication Security
*   **SSH & TLS:** Use Secure Shell for remote server management and TLS for data-in-transit.
*   **CI/CD Hardening:** Lock down GitHub/GitLab repositories. Conduct periodic audits of the delivery pipeline.
*   **Secrets Management:** Never hardcode credentials. Utilize **HashiCorp Vault** or Cloud Secret Managers for keys and certificates.

### D. Monitoring & Detection
*   **Logging:** Centralize logs to detect anomalies (e.g., unauthorized admin login attempts).
*   **Intrusion Detection Systems (IDS):** 
    *   **Endpoint Security:** Protect servers and nodes.
    *   **Network Security:** Utilize tools like `Nmap` and `Snort`.
    *   **System-Call Auditing:** Monitor the Linux kernel for suspicious activity.

---

## 3. Security Design Patterns
Security patterns provide reusable, standardized solutions to recurring threats, ensuring that security is "baked into" the architecture rather than "bolted on."

### Core Attributes of Security Patterns
*   **Traceability:** Controls must be traceable back to specific threats.
*   **Reusability:** Standardized usage across multiple services/applications.
*   **Vendor Agnostic:** Decouple security logic from specific technology implementations.

### Organizing Patterns (The Security Catalog)
1.  **Authentication & Authorization:** (e.g., OAuth2, Role-Based Access Control).
2.  **Boundary Defense:** (e.g., XML Firewalls, Web Application Firewalls).
3.  **Data Integrity:** (e.g., Digital Signatures, SAML).

> [!IMPORTANT]
> Use **Unified Modeling Language (UML)** to visualize security flows and analyze code to understand how specific design patterns impact the overall attack surface.

---

## 4. Modern Encryption: TLS/SSL
Transport Layer Security (TLS) is the industry-standard successor to SSL, providing confidentiality and integrity for client-server communication.

### The TLS Handshake Process
1.  **Version Negotiation:** Client and server agree on the highest supported TLS version.
2.  **Cipher Selection:** Selection of the encryption algorithm (Cipher Suite).
3.  **Authentication:** The client verifies the server’s identity via its TLS Certificate.
4.  **Key Exchange:** Generation of session keys for symmetric encryption.

### SDLC Integration & Maintenance
*   **Certificate Lifecycle:** Automate renewals via CI/CD to prevent expiration outages (ideally every 90 days).
*   **Protocol Hardening:** Deprecate outdated versions (TLS 1.0, 1.1) and enforce strong, modern ciphers.
*   **Auditability:** Regularly scan endpoints to ensure no vulnerable ciphers are exposed to the public internet.

---


# OpenSSL: Engineering Guide to Cryptography & Secure Data Transmission

[![License: Open Source](https://img.shields.io/badge/License-Open%20Source-blue.svg)](https://www.openssl.org/)
[![Focus: Security](https://img.shields.io/badge/Domain-SecOps%20%2F%20DevOps-red.svg)](#)

## 📌 Overview
In modern cloud infrastructure and DevOps pipelines, ensuring data **Confidentiality** and **Integrity** is non-negotiable. This technical guide covers the implementation of **OpenSSL**, a robust, commercial-grade toolkit for Transport Layer Security (TLS) and Secure Sockets Layer (SSL). 

This documentation transitions from foundational cryptographic theory to hands-on command-line implementations for encrypting sensitive configuration files, managing keys, and hardening data against unauthorized interception (Snooping, Spoofing, and Hijacking).

---

## 🛡️ Core Cryptographic Pillars

### 1. Confidentiality & Integrity
* **Confidentiality:** Utilizing cryptographic keys to ensure that data remains unreadable to unauthorized entities. Even if intercepted over a non-secure network, the data remains encrypted.
* **Integrity:** Guarantees that data has not been tampered with during transit. This is typically achieved via **Message Digests** (hashes) and checksums.

### 2. Public Key Cryptography (Asymmetric)
The toolkit leverages **RSA** (Rivest-Shamir-Adleman) for secure key exchange and digital signatures. 
- **Private Key:** Kept secret; used for decryption and signing.
- **Public Key:** Shared openly; used for encryption and verification.

### 3. Message Digest Algorithms
Cryptographic hash functions compute fixed-size checksums of data blocks. These are used to:
- Verify file identity (Checksumming).
- Sign/Verify digital signatures.

---

## 🌐 Security in the OSI Model
Secure communication protocols like **SSL/TLS** primarily operate at the upper layers of the OSI model to protect data before it is transmitted across the network.

| Layer | Protocol / Security Implementation |
| :--- | :--- |
| **Application (L7)** | HTTPS, SSH, Secure API Endpoints |
| **Presentation (L6)** | SSL/TLS Encryption, Data Formatting |
| **Transport (L4)** | TCP/UDP (Secured via TLS handshake) |
| **Network (L3)** | IPsec, VPN Tunnels |

> **DevOps Note:** When building CI/CD pipelines, security must be "shifted left." Code commits, secrets management, and artifact delivery must be protected using the encryption standards detailed below.

---

## 🛠️ Implementation: Secure File Operations
This section demonstrates the CLI-based workflow for securing sensitive files using the **AES-256-CBC** cipher with **PBKDF2** (Password-Based Key Derivation Function 2).

### Task 1: Decrypting Production Secrets
In a scenario where a configuration file is retrieved from remote storage (e.g., S3/Cloud Object Storage), use the following procedure to decrypt it.

```bash
# 1. Retrieve the encrypted artifact
wget [https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-CD0267EN-SkillsNetwork/labs/module1/encrypted_secretfile](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-CD0267EN-SkillsNetwork/labs/module1/encrypted_secretfile)

# 2. Decrypt using AES-256-CBC
# Password for this example: adios
openssl aes-256-cbc -d -a -pbkdf2 -in encrypted_secretfile -out secrets.txt

```

**Flag Breakdown:**

**Flag**

**Description**

`aes-256-cbc`

Advanced Encryption Standard (256-bit key) in Cipher Block Chaining mode.

`-d`

Decrypt mode.

`-a`

Base64 decode the input before decryption.

`-pbkdf2`

Implements PBKDF2 for password-to-key derivation (Security Standard).

`-in`

Input file path.

`-out`

Output destination for the plaintext.

----------

### Task 2: Encrypting sensitive configuration files

To secure a local file before pushing it to a non-secure environment:

Bash

```
# Encrypt secrets.txt with a local password
openssl aes-256-cbc -a -pbkdf2 -in secrets.txt -out secrets.txt.enc

# Securely remove the plaintext file
rm secrets.txt

```

----------

### Task 3: Hardening Encryption (Iteration Scaling)

To mitigate brute-force risks, increase the computational cost of the key derivation by increasing iterations.

Bash

```
# Applying 2500 iterations for PBKDF2 hardening
openssl aes-256-cbc -a -pbkdf2 -iter 2500 -in secrets.txt -out secrets_hardened.txt.enc

```

----------

## 🔍 Advanced Exploration

Engineers should familiarize themselves with the various cipher suites available in their OpenSSL version. To list all supported encryption algorithms:

Bash

```
openssl enc --list
```


---



# SecOps Lifecycle: Threat Modeling, Vulnerability Scanning & Infrastructure Monitoring

![Security Lifecycle](https://img.shields.io/badge/Security-Shift--Left-blueviolet?style=for-the-badge)
![DevSecOps](https://img.shields.io/badge/DevSecOps-Automated-green?style=for-the-badge)
![Compliance](https://img.shields.io/badge/Compliance-OWASP%20%7C%20NIST%20%7C%20CVE-red?style=for-the-badge)

## 📌 Executive Summary
This repository serves as a professional technical guide for implementing **SecOps** principles within the Software Development Lifecycle (SDLC). It moves beyond basic theory to address the practical engineering requirements of **Threat Modeling**, **Automated Scanning**, and **Container Security** within high-scale cloud environments.

---

## 🏗️ 1. Architectural Threat Modeling
Threat modeling is a critical design-phase activity. Instead of reactive patching, we employ proactive architectural analysis to eliminate vulnerabilities before a single line of code is committed.

### Methodologies
| Model | Full Name | Focus | Best For |
| :--- | :--- | :--- | :--- |
| **STRIDE** | Spoofing, Tampering, Repudiation, Info Disclosure, DoS, Elevation of Privilege | Categorizing threats | Application-level security (Microsoft Standard) |
| **PASTA** | Process for Attack Simulation & Threat Analysis | Risk-centric | Aligning business objectives with technical defense |
| **VAST** | Visual, Agile, and Simple Threat | Agile/Automation | Large-scale infrastructure & operational workflows |

> **Engineer's Note:** We utilize **Data Flow Diagrams (DFDs)** during the design phase to identify trust boundaries and potential entry points for lateral movement.

---

## 🔍 2. Automated Vulnerability Management (SAST)
We implement **Static Application Security Testing (SAST)** to analyze source code for common injection flaws (SQLi, XSS) and insecure path traversals.

### Enterprise Tooling Suite
- **Snyk Code:** Real-time semantic analysis integrated directly into the IDE.
- **Coverity:** Incremental analysis for high-performance languages like C++, Java, and Python.
- **CodeSonar:** Advanced path and variable modeling to find complex logical weaknesses.
- **Static Reviewer:** Compliance mapping against **OWASP Top 10**, **CVE**, and **NIST** frameworks.

---

## 🛡️ 3. Continuous Threat Monitoring & Supply Chain Security
Modern security requires monitoring the entire supply chain, including Third-party libraries, SCM repositories, and OCI-compliant containers.

### Repository Hardening (SCM Security)
- **Automated Fix PRs:** Tools like Dependabot or Snyk automatically generate pull requests for vulnerable dependencies.
- **Commit Verification:** Mandatory **PGP/GPG signing** for all commits to ensure identity non-repudiation.
- **Secret Scanning:** Prevention of password, API key, and credential leakage in the codebase.

### Container & Artifact Security
Containers are not just code; they are layered environments. Our scanning covers:
1. **Base Image Analysis:** Scanning the parent OS (Alpine, Debian, etc.).
2. **Layer Analysis:** Checking every `RUN` command in the Dockerfile for misconfigurations.
3. **Dependency Scanning:** Auditing bundled libraries within the application package.

---

## 🔐 4. Core Security Foundations
A standardized vocabulary is essential for engineering communication.

### Authentication (AuthN) vs. Authorization (AuthZ)
- **AuthN:** Verifying *who* the user is (e.g., OIDC, SAML, Biometrics).
- **AuthZ:** Determining *what* the user can do (e.g., RBAC, ABAC).

### Cryptographic Standards
- **Data Integrity:** Utilizing **Secure Hash Algorithms (SHA-256/512)** to generate digital fingerprints. If the hash changes, the data is compromised.
- **Encryption Patterns:**
    - **Symmetric:** Fast, single-key (AES-256) used for bulk data encryption.
    - **Asymmetric:** Public/Private key pairs (RSA/ECC) used for secure key exchange and signatures.

---

## 🚀 5. Implementing Security in CI/CD (DevSecOps)
Security is not a gate; it is a continuous pipeline.

1. **DEVELOP:** IDE plugins provide immediate feedback (Linting + Security).
2. **TEST:** SAST and DAST (Dynamic Scanning) run alongside unit/integration tests.
3. **DEPLOY:** Container images are scanned in the Registry before being pulled by Kubernetes.
4. **OPERATE:** Runtime security monitoring detects anomalies in production environments.
```yaml
# Example Security Job for a CI Pipeline
security_scan:
  stage: test
  image: snyk/snyk-cli:docker
  script:
    - snyk auth $SNYK_TOKEN
    - snyk test --severity-threshold=high
    - snyk monitor # Continuous monitoring of the production branch

```

----------

## 📑 Compliance Frameworks

Our security posture is mapped against global standards to ensure regulatory compliance:

-   **OWASP:** For web-tier vulnerability mitigation.
    
-   **CVE (Common Vulnerabilities and Exposures):** For standardized threat identification.
    
-   **NIST:** For risk management and infrastructure framework.


---


# Network Reconnaissance & Security Infrastructure Toolkit

A comprehensive guide and professional reference for **Nmap**, **OpenSSL**, and essential Linux infrastructure utilities. This repository serves as a technical manual for Security Engineers, DevOps practitioners, and System Administrators focusing on network auditing and data integrity.

---

## 1. Network Reconnaissance with Nmap (Network Mapper)

Nmap is the industry-standard open-source tool for network discovery and security auditing. It is designed to rapidly scan large networks, although it works fine against single hosts.

### 1.1 Core Objectives
*   **Host Discovery:** Identifying active nodes on a network.
*   **Service Enumeration:** Discovering open ports and identifying the services (application name and version) running on them.
*   **OS Fingerprinting:** Determining the operating system and hardware characteristics of network devices.
*   **Vulnerability Assessment:** Utilizing the Nmap Scripting Engine (NSE) to detect misconfigurations or known CVEs.

### 1.2 Professional Use Cases
| Role | Application |
| :--- | :--- |
| **Security Analysts** | Assessing attack surfaces and identifying unauthorized services. |
| **DevOps/SRE** | Validating firewall rules and load balancer configurations in CI/CD pipelines. |
| **System Admins** | Troubleshooting connectivity issues and auditing network inventory. |
| **Compliance Auditors** | Verifying adherence to security standards (PCI-DSS, HIPAA) regarding exposed ports. |

### 1.3 Technical Scan Reference
Professional scanning requires choosing the right technique based on the environment (IDS/IPS presence, network latency).

| Scan Type | Flag | Description | Engineering Context |
| :--- | :--- | :--- | :--- |
| **TCP Connect** | `-sT` | Completes the 3-way handshake. | Used when the user lacks raw socket privileges. |
| **SYN Stealth** | `-sS` | "Half-open" scan; does not complete connections. | Standard for speed and bypassing simple filters. |
| **UDP Scan** | `-sU` | Scans for DNS, DHCP, and SNMP services. | Critical for a full surface analysis. |
| **Service Version** | `-sV` | Probes open ports to determine service versions. | Essential for vulnerability mapping. |
| **OS Detection** | `-O` | Analyzes TCP/IP stack fingerprints. | Useful for identifying legacy or unpatched systems. |
| **Aggressive Scan** | `-A` | OS detection, versioning, script scanning, and traceroute. | Comprehensive "all-in-one" scan for deep analysis. |

> **Warning:** Unauthorized scanning can be interpreted as an attack. Always ensure you have written permission (Rules of Engagement) before scanning networks you do not own.

---

## 2. Practical Operation: Zenmap Workflow

Zenmap provides a GUI for Nmap, allowing for visual topology mapping and easier result aggregation.

### Operational Procedure
1.  **Target Selection:** Input the FQDN (e.g., `scanme.nmap.org`) or CIDR range (e.g., `192.168.1.0/24`).
2.  **Profile Selection:**
    *   `Intense Scan`: Full enumeration for deep-dive security audits.
    *   `Quick Scan`: Rapid identification of common ports (e.g., 80, 443, 22).
3.  **Analysis Tabs:**
    *   **Nmap Output:** Raw logs for automated processing.
    *   **Ports/Hosts:** Visual state of service availability.
    *   **Topology:** Graphical representation of the network hops and node relationships.
    *   **Host Details:** Aggregated metadata including uptime, OS guesses, and hardware addresses.

---

## 3. Cryptography & Data Integrity: OpenSSL

OpenSSL is a robust commercial-grade toolkit for the Transport Layer Security (TLS) and Secure Sockets Layer (SSL) protocols.

### 3.1 Encryption at Rest (Symmetric)
For securing sensitive artifacts (images, configuration files, backups) using the AES-256-CFB algorithm.

*   **Encryption Command:**
    ```bash
    openssl enc -aes-256-cfb -salt -in backup.tar.gz -out backup.tar.gz.enc -k $SECRET_PASSPHRASE
    ```
*   **Decryption Command:**
    ```bash
    openssl enc -d -aes-256-cfb -in backup.tar.gz.enc -out backup.tar.gz -k $SECRET_PASSPHRASE
    ```

### 3.2 Key Utility Commands
*   **Help & Documentation:** `openssl -help`
*   **List Supported Ciphers:** `openssl enc -list`

---

## 4. Infrastructure & Automation Utilities

Essential CLI tools for managing remote artifacts and maintaining environment hygiene in Linux-based cloud environments.

### 4.1 Remote Artifact Retrieval (`wget`)
Non-interactive network downloader used for fetching binaries, configuration files, or logs.

*   **Standard Download:**
    ```bash
    wget [https://enterprise-repo.local/v1/config.yaml](https://enterprise-repo.local/v1/config.yaml)
    ```
*   **Background Operation:** (Useful for large datasets in remote shells)
    ```bash
    wget -b [https://mirror.kernel.org/distros/large-iso.iso](https://mirror.kernel.org/distros/large-iso.iso)
    ```

### 4.2 Filesystem Cleanup (`rm`)
Used for purging temporary files or old logs within automation scripts.

*   **Recursive Cleanup:**
    ```bash
    # Removes directory and all sub-contents recursively
    rm -rf ./tmp/build-artifacts/
    ```
*   **Interactive Removal:**
    ```bash
    # Prompts for confirmation (safety measure for production)
    rm -i critical_config.json
    ```

---

## 5. Summary Table for Quick Reference

| Tool | Primary Use | Level |
| :--- | :--- | :--- |
| **Nmap** | Network Mapping / Security Auditing | Professional |
| **OpenSSL** | Encryption / Certificate Management | Expert |
| **Wget** | Automated Data Retrieval | Foundation |
| **Rm** | Resource Management / Cleanup | Foundation |

---





# DevSecOps: Static Application Security Testing (SAST) Implementation with SonarQube

[![Security: SAST](https://img.shields.io/badge/Security-SAST-blueviolet)](https://owasp.org/www-community/Source_Code_Analysis_Tools)
[![Tool: SonarQube](https://img.shields.io/badge/Tool-SonarQube-informational)](https://www.sonarqube.org/)
[![Environment: Docker](https://img.shields.io/badge/Environment-Docker-blue)](https://www.docker.com/)

## 📌 Overview
This repository documents the professional implementation of **Static Application Security Testing (SAST)** within a modern **SDLC (Software Development Life Cycle)**. By leveraging static analysis, we "Shift Left" our security posture, identifying vulnerabilities and code smells before the execution phase.

This guide covers the containerized deployment of a **SonarQube** ecosystem and the execution of security scans on a Python-based microservice.

---

## 🚀 Key Benefits of Static Analysis

> "Quality is not an act, it is a habit. In DevSecOps, static analysis is the automated guardrail that ensures code quality at scale."

| Benefit | Description |
| :--- | :--- |
| **Depth** | Scans execution paths that manual reviews or functional tests might miss. |
| **Speed** | Integrated into the **Develop** stage to provide near-instant feedback to engineers. |
| **Accuracy** | Utilizes updated vulnerability databases (OWASP Top 10, CWE) to minimize false negatives. |
| **Cost-Efficiency** | Identifying a vulnerability in the IDE costs significantly less than fixing a breach in Production. |

---

## 🛠 Infrastructure Setup (Containerized)

We utilize **Docker** to ensure an isolated and reproducible environment. The setup includes a persistent PostgreSQL backend and the SonarQube core engine.

### 1. Network Provisioning
Create a dedicated bridge network for inter-container communication:
```bash
docker network create mynet

```

### 2. Database Deployment (PostgreSQL)

Provision the persistent storage layer for SonarQube metadata:

Bash

```
docker run --name postgres \
  -e POSTGRES_USER=root \
  -e POSTGRES_PASSWORD=Test12345 \
  -p 5432:5432 \
  --network mynet \
  -d postgres

```

### 3. Application Server Deployment (SonarQube)

Launch the SonarQube community instance linked to the DB:

Bash

```
docker run -d --name sonarqube \
  -p 9000:9000 \
  -e sonar.jdbc.url=jdbc:postgresql://postgres/postgres \
  -e sonar.jdbc.username=root \
  -e sonar.jdbc.password=Test12345 \
  --network mynet \
  sonarqube

```

----------

## 🖥️ Configuration & Authentication

Access the dashboard via `http://localhost:9000`.

**Default Credentials:**

**Attribute**

**Value**

**Username**

`admin`

**Password**

`admin` (Update required on first login)

### Scanner Setup

To avoid local environment pollution, we use the **sonar-scanner-cli** Docker image. Set up an alias for professional-grade CLI interaction:

Bash

```
# Pull the latest scanner image
docker pull sonarsource/sonar-scanner-cli

# Establish an alias for local source mounting
alias sonar-scanner='docker run --rm -v "$(pwd):/usr/src" sonarsource/sonar-scanner-cli'

```

----------

## 🔍 Security Scan Execution

### 1. Target Repository Acquisition

Clone the target Python microservice for analysis:

Bash

```
git clone [https://github.com/ibm-developer-skills-network/wtecc-CICD_PracticeCode.git](https://github.com/ibm-developer-skills-network/wtecc-CICD_PracticeCode.git)
cd wtecc-CICD_PracticeCode

```

### 2. Triggering the Analysis

Execute the scan using your generated project token (replace placeholders):

Bash

```
sonar-scanner \
  -Dsonar.projectKey=microservice-audit \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=[YOUR_PROJECT_TOKEN]

```

----------

## 📊 Vulnerability Report & Remediation

### Identified Issue: CSRF Vulnerability

The analysis flagged a **Security Hotspot** regarding **Cross-Site Request Forgery (CSRF)** in the Flask implementation.

**Risk Assessment:**

-   **Vulnerability:** Lack of CSRF protection on state-changing requests.
    
-   **Impact:** Attackers could perform unauthorized actions on behalf of authenticated users.
    

**Remediation Strategy (Flask):**

To mitigate this, implement `Flask-WTF` or `CSRFProtect`:

Python

```
from flask import Flask
from flask_wtf.csrf import CSRFProtect

app = Flask(__name__)
csrf = CSRFProtect()
csrf.init_app(app) # Security middleware initialized
```


---




# Dynamic Application Security Testing (DAST) with OWASP ZAP

[![Security: DAST](https://img.shields.io/badge/Security-DAST-red.svg)](#)
[![Tool: OWASP ZAP](https://img.shields.io/badge/Tool-OWASP%20ZAP-blue.svg)](#)
[![Environment: Docker](https://img.shields.io/badge/Environment-Docker-blue.svg)](#)

---

## 🛠 Core Concepts

### Why Dynamic Analysis?
* **Runtime Context:** Identifies issues like insecure server configurations and authentication flaws that static tools miss.
* **Attack Simulation:** Acts as a malicious actor to test the application's actual response to payloads.
* **False Positive Reduction:** Since the tool interacts with a running instance, findings often represent exploitable vulnerabilities rather than theoretical code smells.

### DAST vs. SAST Comparison
| Feature | Static Analysis (SAST) | Dynamic Analysis (DAST) |
| :--- | :--- | :--- |
| **View** | Inside-Out (White-box) | Outside-In (Black-box) |
| **Requirement** | Source Code / Binaries | Running Application |
| **Timing** | Early Development (Code/Build) | Staging / Pre-Prod / Prod |
| **Identifies** | Syntax, Logic, Hardcoded Secrets | Server Config, XSS, Session Issues |

---

## 🚀 Implementation Guide

### Phase 1: Deploying the Target Environment
We utilize the **OWASP Juice Shop** as our target. It is deployed via Docker to ensure environment parity and isolation.

```bash
# Pull the latest Juice Shop image
docker pull bkimminich/juice-shop

# Deploy the application on port 3000
docker run --rm -p 3000:3000 bkimminich/juice-shop

```

_Wait for the log: `info: Server listening on port 3000`_

### Phase 2: Executing the DAST Scanner

We use the **OWASP ZAP Stable** dockerized image to perform a `zap-baseline` scan. This script crawls the target and identifies common security gaps.

Bash

```
# Pull the OWASP ZAP security tool
docker pull softwaresecurityproject/zap-stable

# Execute the baseline scan against the Target URL
# Replace {TARGET_URL} with your instance endpoint
docker run -t softwaresecurityproject/zap-stable zap-baseline.py -t {TARGET_URL}

```

### Phase 3: Analyzing the Attack Surface

The scan generates a report categorizing findings into `PASS`, `WARN`, and `FAIL`.

#### ⚠️ Critical Findings Summary

**Vulnerability ID**

**Alert Type**

**Risk Level**

**Impact**

**10017**

Cross-Domain JS Inclusion

Medium

Potential Data Leakage

**10020**

Missing Anti-clickjacking

Medium

UI Redressing Attacks

**10038**

Content Security Policy (CSP)

High

XSS / Code Injection

**90033**

Loosely Scoped Cookie

Low

Session Hijacking

----------

## 🔍 Remediation Workflow

A professional engineer does not just find bugs; they manage the lifecycle of the fix. For every `WARN` or `FAIL` found by ZAP:

1.  **Reference:** Use the ZAP Alert ID to find detailed documentation:
    
    `https://www.zaproxy.org/docs/alerts/{ID}`
    
2.  **Validate:** Manually verify the endpoint listed in the scan results.
    
3.  **Patch:** Update HTTP headers (e.g., `X-Content-Type-Options`, `Content-Security-Policy`) or modify application logic.
    
4.  **Regression:** Re-run the ZAP container to ensure the status moves to `PASS`.
    

----------

## 🏗 DevOps Integration (CI/CD)

To automate this in a real-world infrastructure, integrate ZAP as a step in your Jenkins/GitHub Actions/GitLab CI:

YAML

```
# Example Pipeline Logic (Conceptual)
security_scan:
  stage: test
  script:
    - docker run -t softwaresecurityproject/zap-stable zap-baseline.py -t $STAGING_URL -r report.html
  artifacts:
    paths: [report.html]
```




---


# DevSecOps: Vulnerability Analysis & Software Composition Analysis (SCA)

![Security](https://img.shields.io/badge/Security-DevSecOps-red)
![Audit](https://img.shields.io/badge/Tooling-pip--audit%20%7C%20OWASP%20ZAP-blue)
![Compliance](https://img.shields.io/badge/Compliance-NVD%20%7C%20CVE-green)

## 📌 Executive Summary
In a modern Software Development Lifecycle (SDLC), security is not a final checkpoint but a continuous integration process. This repository documents the methodologies for evaluating software vulnerabilities through **Static Analysis**, **Dynamic Analysis (DAST)**, and **Software Composition Analysis (SCA)**. 

The goal is to move from reactive patching to a proactive security posture by identifying flaws in proprietary code and third-party dependencies before they reach production.

---

## 🛠 Tech Stack & Tooling
| Category | Tools / Frameworks |
| :--- | :--- |
| **SCA Tooling** | `pip-audit`, OWASP Dependency-Check |
| **DAST Scanning** | OWASP ZAP (Zed Attack Proxy) |
| **Environment** | Python 3.x, Virtualenv, Flask |
| **Defect Tracking** | Jira, Bugzilla |
| **Standards** | OWASP Top 10, NVD (National Vulnerability Database) |

---

## 🧠 Core Concepts

### 1. Software Composition Analysis (SCA)
Modern applications are built using ~80% open-source libraries. SCA allows engineers to:
* **Identify** all third-party components and their versions.
* **Cross-reference** dependencies against the **National Vulnerability Database (NVD)** for known CVEs.
* **Audit Licenses** to ensure legal compliance and avoid "copyleft" risks.

### 2. Dynamic Application Security Testing (DAST)
Using tools like **OWASP ZAP**, we perform automated "Black Box" testing on running applications to identify runtime vulnerabilities such as:
* **XSS (Cross-Site Scripting)**
* **Anti-CSRF Token Absence**
* **Insecure HTTP Headers** (Missing CSP, HSTS, or Clickjacking protection)
* **Information Disclosure** (Server version leaks)

### 3. Vulnerability Remediation Workflow
Vulnerabilities are prioritized based on business impact:
1.  **Mission-Critical:** Immediate patch required (Blocker).
2.  **High:** Resolve in the next sprint.
3.  **Medium/Low:** Backlog for scheduled maintenance.

---

## 🚀 Hands-on Implementation: Dependency Auditing

### Phase 1: Environment Provisioning
Isolate the environment to prevent dependency conflicts and ensure a clean audit trail.

```bash
# Update system and install virtual environment support
sudo apt-get update && sudo apt-get install -y python3-venv

# Initialize and activate the virtual environment
python3 -m venv venv
source venv/bin/activate

# Verify environment isolation
which python  # Expected: /home/project/venv/bin/python

```

### Phase 2: Tooling Installation

Install `pip-audit`, a tool specifically designed to scan Python environments for known vulnerabilities.

Bash

```
pip install pip-audit
pip-audit --help

```

### Phase 3: Analyzing a Vulnerable Target (Hit Counter App)

We use a sample Flask application with outdated dependencies to simulate a production "Technical Debt" scenario.

Bash

```
# Clone the target microservice
git clone [https://github.com/ibm-developer-skills-network/ycuer-flask-hitcounter.git](https://github.com/ibm-developer-skills-network/ycuer-flask-hitcounter.git)
cd ycuer-flask-hitcounter

# Install dependencies (simulating a legacy deployment)
pip install -r requirements.txt

# Execute the Vulnerability Scan
pip-audit

```

> [!IMPORTANT]
> 
> **Interpreting Results:** `pip-audit` will generate a report listing the Package Name, Installed Version, and associated **CVE ID**. If a vulnerability is found (e.g., in Flask 1.1.4), the remediation step is to update the `requirements.txt` to a patched version and re-audit.

----------

## 🔍 Dynamic Analysis Demonstration (OWASP ZAP)

To perform an automated scan on a live endpoint:

1.  **Initialize:** Launch OWASP ZAP and select **Automated Scan**.
    
2.  **Targeting:** Provide the URL (e.g., `http://testphp.vulnweb.com`).
    
3.  **Attack:** Trigger the spider and active scan.
    
4.  **Analysis:** Review the **Alerts** tab for high-risk findings.
    

### Common Findings Table

**Alert**

**Risk Level**

**Description**

**XSS**

High

User-controllable HTML elements allowing script injection.

**Missing Anti-CSRF**

Medium

Potential for unauthorized actions on behalf of a user.

**Server Leaks**

Low

Header fields exposing server versions (e.g., `Server: Apache/2.4.1`).

----------

## 🛡 Security Governance & Bug Bounties

When an organization scales beyond the capacity of internal security teams:

-   **Defect Tracking:** Use **Jira** to centralize vulnerability management and assign ownership to developers.
    
-   **Bug Bounty Programs:** Implement internal or public programs (e.g., via HackerOne) to crowdsource vulnerability discovery. This incentivizes external researchers to find and report bugs ethically in exchange for rewards.

---


# DevSecOps: Security Testing & Mitigation Framework

![Build Status](https://img.shields.io/badge/Security-Verified-brightgreen)
![Platform](https://img.shields.io/badge/Environment-Linux%20%7C%20Docker%20%7C%20Cloud-blue)

A comprehensive technical guide and toolkit for implementing **Security Testing and Mitigation Strategies** within modern CI/CD pipelines. This repository covers automated scanning, container security, and environment hardening.

---

## 🛠 Tech Stack & Core Tooling

| Category | Tools | Purpose |
| :--- | :--- | :--- |
| **Automation** | Bash, Jake | Shell scripting and build orchestration |
| **SCA** | OWASP Dependency-Check | Identification of vulnerable dependencies |
| **SAST** | SonarScanner | Static application security testing |
| **Containerization** | Docker | Environment isolation and network security |
| **Data Processing** | jq | High-performance JSON parsing for scan reports |
| **Package Mgmt** | pip | Python dependency management and audits |

---

## 🐚 1. Advanced Shell Automation (Bash)

Bash is the primary interpreter for security automation scripts. We focus on idempotent scripts and environment efficiency.

### High-Frequency Aliases
Optimize your workflow by aliasing complex security commands or frequent directory navigation.
```bash
# Registering aliases for security contexts
alias scan-report="cd /var/reports/security/vulnerabilities"
alias docker-clean="docker network prune -f && docker container prune -f"

# View active configurations
alias -p

```

### Automation Logic

Example of a health-check loop used to monitor service availability during a mitigation deployment:

Bash

```
#!/bin/bash
# Monitoring service availability for 3 iterations
for i in {1..3}; do
    echo "[LOG] Health Check Attempt: $i"
    # Logic to verify service status
    sleep 1
done

```

----------

## 🛡️ 2. Security Scanning & Analysis

### OWASP Dependency-Check (SCA)

Identify publicly disclosed vulnerabilities within your project's third-party libraries.

**Implementation (Linux/CI):**

Bash

```
dependency-check.sh --project "Production_API" \
                    --scan "/app/dependencies/lib" \
                    --out "/app/reports/sca_results" \
                    --format "HTML"

```

### SonarScanner (SAST)

Execute deep code analysis to detect security hotspots and code smells.

Bash

```
# Execute scan with debug logging for CI troubleshooting
sonar-scanner -Dsonar.projectKey=SecureApp_v2 -X

```

----------

## 🐳 3. Container Security & Networking

Efficient container management is critical for minimizing the attack surface.

### Network Isolation

Always isolate application tiers using dedicated Docker networks.

Bash

```
# Create a dedicated secure bridge network
docker network create internal_api_net

# Inspect network for unauthorized containers
docker network inspect internal_api_net

# Connect a hardened container to the network
docker network connect internal_api_net secure_app_container

```

### Container Lifecycle

Bash

```
# List all active security containers
docker ps -a

# Pull the latest hardened images from a private registry
docker pull security-hardened-alpine:latest

# Run a container with restricted resource allocation
docker run -d --name secure_agent security-hardened-alpine

```

----------

## 📊 4. Utility & Data Transformation

### `jq` for Report Parsing

Security scanners often output large JSON files. `jq` is used to filter high-severity vulnerabilities for automated alerts.

Bash

```
# Extracting specific 'vulnerability_id' from a scan report
jq '.[].vulnerability_id' scan_results.json

# Validating key existence in security metadata
jq 'map(has("cve_score"))' metadata.json

```

### `wget` for Artifact Acquisition

Securely retrieve patches or security binaries in non-interactive environments.

Bash

```
# Download security definitions in background mode
wget -b [https://security-updates.internal/vulnerability-db.zip](https://security-updates.internal/vulnerability-db.zip)

# Retrieve specific config files
wget [https://raw.githubusercontent.com/cis-benchmarks/config/main/linux-hardened.conf](https://raw.githubusercontent.com/cis-benchmarks/config/main/linux-hardened.conf)

```

----------

## 🔍 5. System Introspection

Identify the environment context and verify the paths of security binaries to prevent "Path Hijacking" attacks.

Bash

```
# Locate multiple binary paths to ensure the correct version is executed
which -a python java

# Verify the path of the current shell executable
which bash

```

----------

## 🚀 6. Build Orchestration (`Jake`)

`Jake` acts as the JavaScript-based build tool to automate repetitive tasks like running the **DDT (Data-Driven Testing)** suite.

Bash

```
# Execute the Data-Driven Testing task
jake ddt

```

----------

> **Engineer's Note:** Always ensure that `pip list` is audited against known CVEs before deploying Python-based security agents. Use `pip install` only with pinned versions to prevent dependency confusion attacks.

Bash

```
# Audit current environment packages
pip list
```
