# 🛒 E-Commerce Microservices Architecture (MERN Stack)

![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white)
![NodeJS](https://img.shields.io/badge/node.js-6DA55F?style=for-the-badge&logo=node.js&logoColor=white)
![React](https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB)
![MongoDB](https://img.shields.io/badge/MongoDB-%234ea94b.svg?style=for-the-badge&logo=mongodb&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)
![SSL](https://img.shields.io/badge/SSL-Let's_Encrypt-blue?style=for-the-badge&logo=letsencrypt&logoColor=white)

A production-ready, containerized E-Commerce application utilizing the MERN stack. This project demonstrates a scalable Microservices architecture, deployed on AWS EC2, orchestrated by Docker Compose, and secured with a host-level Nginx Reverse Proxy and SSL (HTTPS).

---

## 🎯 The Problem

Traditional monolithic applications face significant challenges in modern deployment environments:
1. **Scalability:** Inability to scale frontend and backend independently based on traffic demands.
2. **Environment Inconsistency:** "It works on my machine" syndrome due to differing development and production environments.
3. **Security & Routing:** Exposing backend APIs directly to the internet increases the attack surface and causes CORS (Cross-Origin Resource Sharing) complexities.
4. **Complex Deployment:** Manual provisioning and deployment processes that are error-prone and time-consuming.

## 💡 The Solution (Our Outcome)

We overcame these challenges by implementing a modern, containerized architecture:

* **Containerization (Docker):** Isolated the Frontend (React) and Backend (Node.js) into independent containers, ensuring 100% parity across all environments.
* **Multi-Stage Builds:** Optimized the frontend Docker image by separating the build process from the serving phase, utilizing a lightweight Nginx Alpine image to serve the static assets.
* **Internal Networking (Docker Compose):** Created a private Docker bridge network (`app-network`). The database (MongoDB/Redis) and the Backend are hidden from the public internet and communicate securely using container DNS.
* **Host-Level Reverse Proxy (Nginx):** Deployed a primary Nginx server on the host OS to act as the single entry point. It handles incoming traffic, serves as an SSL terminator, and forwards requests to the internal Docker network.
* **Zero-Downtime SSL:** Automated HTTPS certificate generation and renewal using Let's Encrypt and Certbot directly on the host machine.

---

## 🛠️ Technologies & Tools

### Development Stack (MERN)
* **Frontend:** React.js, Vite, Tailwind CSS
* **Backend:** Node.js, Express.js
* **Database:** MongoDB (Persistent volume), Redis (Caching)

### DevOps & Infrastructure
* **Cloud Provider:** AWS (EC2 Instance)
* **Containerization:** Docker, Docker Compose
* **Web Server / Proxy:** Nginx (Containerized for frontend + Host-level for reverse proxy)
* **Security:** Let's Encrypt (Certbot) for SSL/TLS
* **DNS:** DuckDNS

---

## 🏗️ Architecture Overview

1. User visits `https://your-domain.com`.
2. The request hits the **Host Nginx** (SSL Terminator on Port 443).
3. The Host Nginx forwards the traffic to the **Frontend Container** (mapped to port 8080).
4. Inside the Frontend container, the **Internal Nginx**:
   - Serves the static React files for root (`/`) requests.
   - Proxies `/api/*` requests internally to the **Backend Container** (Port 5000) using Docker's internal network.
5. The Backend securely communicates with **MongoDB** and **Redis** containers.

---

## 🚀 Getting Started (Deployment Guide)

### Prerequisites
* An AWS EC2 instance (Ubuntu)
* A registered Domain Name pointing to your EC2 Public IP
* Ports `80` (HTTP), `443` (HTTPS), and `22` (SSH) opened in AWS Security Groups.

### Step 1: Clone the Repository
```bash
git clone <your-repo-url>
cd ecommerce
```

### Step 2: Configure Environment Variables
Ensure your `backend/.env` file or Docker Compose environment variables are correctly set (e.g., MongoDB URIs, Stripe Keys, Cloudinary credentials).

### Step 3: Build and Run Containers
```bash
# Ensure Docker and Docker Compose plugin are installed
sudo docker compose up -d --build
```
*Note: The frontend is mapped to port `8080` to leave port `80` free for the host Nginx.*

### Step 4: Setup Host Nginx Reverse Proxy
Install Nginx on the host machine and create a server block to forward traffic to `localhost:8080`.
```bash
sudo apt update && sudo apt install nginx -y
# Link your configuration and restart Nginx
sudo nginx -t && sudo systemctl restart nginx
```

### Step 5: Secure with Let's Encrypt (SSL)
```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d your-domain.com
```
## 🔄 CI/CD Workflow

1. Code pushed to GitHub
2. CI/CD pipeline triggered
3. Docker image built & pushed
4. Kubernetes deployment updated
5. System monitored in real-time

---

## 📊 Monitoring & Observability

* Prometheus collects metrics
* Grafana visualizes system performance
* Helps detect failures and performance issues

---

## 👨‍💻 Author
Built with a DevOps mindset by **Abdullrahman Eissa**.
*Passionate about Infrastructure, Automation, and Secure Deployments.*
```

