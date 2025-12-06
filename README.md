# Home Server – Dockerized Service Stack

This repository contains the configuration files and Docker Compose stack for my fully containerised home server environment.  
All services run in isolated Docker containers for security, modularity, and easy redeployment.  
The goal of this setup is to keep all core services portable, reproducible, and simple to maintain.

---

## Server Specifications

**Hardware:** Dell OptiPlex 7080 Micro  
**CPU:** Intel Core i5-10500T @ 2.30GHz  
**Memory:** 16GB DDR4  
**Storage:** 256GB SSD  
**OS:** Ubuntu Server 24.04 LTS  

---

## Features

- **Dockerized Containers** – Each service runs in its own container, providing maximum isolation and security.

- **Persistent Storage** – All important data, including Pi-hole databases, Minecraft worlds, Samba shares, and Dashy configuration, are stored on persistent volumes, ensuring data survives container updates or recreations.

- **Centralized Management** – Portainer and Nginx Proxy Manager provide easy control and monitoring of your services.

- **Local APIs and Keys** – Sensitive credentials like OpenAI API keys are stored in environment files (`.env`) and not committed to the repository for security.

---

## Overview of Services

### **Pi-hole**  
Provides DNS-level ad blocking and content filtering for the entire network.  
Runs in host network mode for minimal latency and maximum compatibility.

### **OpenWebUI (Local ChatGPT Interface)**  
A self-hosted OpenAI-compatible chat interface.  
Uses real OpenAI API keys supplied via environment variables and offers a local ChatGPT-style dashboard accessible on the network.

### **Nagios**  
Monitors network devices, server health, and service uptime.  
Useful for getting statistics, alerts, and performance data across the home network.

### **Portainer**  
Web-based Docker management UI for controlling and monitoring containers, volumes, and images.

### **Minecraft Server (Modded 1.21 Bukkit)**  
A persistent, modded Bukkit server with whitelist and plugins stored on mounted local storage.  
Runs in its own isolated container for clean updates and easy backups.

### **Samba (NAS)**  
Provides Windows-compatible file sharing over the network.  
Backed by external drives that are mounted into the Samba container.

### **FileBrowser**  
Web-based file manager for browsing drives exposed by the Samba container.  
Offers easy drag-and-drop file management through the browser.

### **Dashy**  
A customizable start-page dashboard that provides quick access links to all hosted services and tools.

### **NGINX + NGINX Proxy Manager**  
Handles reverse proxying, local domain routing, SSL, and exposed web services.  
NPM simplifies domain management, while standalone nginx is available for custom configurations.

---

## Getting Started

Clone this repository:

```bash
git clone git@github.com:Ridge19/home-server.git
cd home-server
```

Create a .env file to store sensitive API keys:

```
OPENAI_API_KEY=your_openai_api_key_here
GEMINI_API_KEY=your_gemini_api_key_here
```

get your API keys at:
- https://platform.openai.com/docs/api-reference/introduction
- https://ai.google.dev/gemini-api/docs
---

## Project Structure
📂 **home/Docker/**
├── 📂 **data/**  
├── 📄 **docker-compose.yml** — Main orchestration file  
├── 📂 **etc-dnsmasq.d/** — DNSMasq configs for Pi-hole  
├── 📂 **etc-pihole/** — Pi-hole non-sensitive configuration files  
├── 📂 **minecraft/**  
├── 📂 **nagios_config/** — Nagios monitoring configuration  
├── 📂 **nginx/** — Custom NGINX configuration  
├── 📂 **nginx-proxy-manager/** — Data and certificates  
└── 📂 **openwebui/**  
Runtime-generated data (databases, caches, etc.) is intentionally excluded.

---

## Access Services

Access services via the following ports (replace `<SERVER_IP>` with your server IP):

- **Pi-hole:** [http://<SERVER_IP>/](http://<SERVER_IP>/)
- **OpenWebUI:** [http://<SERVER_IP>:8080](http://<SERVER_IP>:8080)
- **Nagios:** [http://<SERVER_IP>:8081](http://<SERVER_IP>:8081)
- **Portainer:** [https://<SERVER_IP>:9443](https://<SERVER_IP>:9443)
- **Minecraft:** Connect via port 30000 using your public IP address. Check your public IP here: [https://ipv4.icanhazip.com/](https://ipv4.icanhazip.com/)
- **FileBrowser:** [http://<SERVER_IP>:8082](http://<SERVER_IP>:8082)
- **Dashy:** [http://<SERVER_IP>:8085](http://<SERVER_IP>:8085)
- **NGINX Proxy Manager:** [http://<SERVER_IP>:8181](http://<SERVER_IP>:8181)

## Persistent Volumes

- **portainer_data** – Stores Portainer configuration.  
- **minecraftdata** – Stores Minecraft world and plugin data.  
- **samba** – Stores shared NAS data.  
- **etc-pihole** – Stores Pi-hole configurations and databases.  

---

## Deployment

To start all services:

```bash
docker compose up -d
```

To stop all services:

```bash
docker compose down
```

