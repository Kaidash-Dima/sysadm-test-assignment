# sysadm-test-assignment

# Trading Application — High Availability Architecture

## 📘 Overview
This repository contains the **infrastructure design** for a **high-availability trading platform** capable of handling **10,000 transactions per second** with strict **< 1 ms latency** between the application and the database.

The goal was to design a resilient, fault-tolerant system that ensures **maximum uptime (≤ 5 minutes of downtime per month)** and **data replication across two geographic regions**.

---

## 🏗️ Architecture Summary
The architecture is built around **AWS managed services** to minimize operational overhead while maintaining high performance and reliability.

### Key features:
- Active-active setup across **two Availability Zones (Multi-AZ)**  
- Cross-region disaster recovery via **RDS Read Replica**
- Autoscaling and stateless application tier  
- Centralized logging, monitoring, and alerting  
- Secure and encrypted data flow at every layer  

---

## 🧩 Components

### **1. Compute / Application Layer**
- **Amazon EC2 Auto Scaling Group** or **Amazon EKS (Kubernetes)** — scalable stateless app layer  
- **AWS Application Load Balancer (ALB)** — traffic distribution, retries, and health checks  
- **AWS WAF** — protection against DDoS and OWASP Top-10 attacks  
- **pgBouncer** — database connection pooling for high concurrency  
- **Redis (ElastiCache)** — in-memory cache for sessions, queues, and low-latency access  

### **2. Database Layer**
- **Amazon RDS for PostgreSQL (Multi-AZ)**  
  - Primary Writer in AZ-A  
  - Synchronous Standby in AZ-B (automatic managed failover)  
- **Cross-Region Read Replica**  
  - Asynchronous replication for ETL, reporting, and DR  
  - Can be promoted to Writer during regional outage  

### **3. Networking & Security**
- **Amazon Route 53** — DNS-based failover and global health checks  
- **AWS VPC** — private/public subnets, Security Groups, NACLs  
- **AWS KMS** — encryption at rest and in transit  
- **TLS termination** via **ACM certificates**  

### **4. Observability**
- **Prometheus** — application and DB metrics  
- **Grafana** — dashboards (TPS, latency p95/p99, replication lag)  
- **Alertmanager** — incident alerts and escalation  
- **Loki** — centralized log aggregation  

### **5. Backup & DR**
- Automated **RDS Snapshots** and **Point-in-Time Recovery (PITR)**  
- **Cross-Region Snapshot Copy** for off-site resilience  
- **DNS failover** for regional outage (Route 53)  

---

## 🗺️ Diagram
Architecture diagram (PlantUML format):

