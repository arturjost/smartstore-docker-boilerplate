# Smartstore Docker Environment (Traefik + PostgreSQL)
🐳 Docker Compose setup for a local Smartstore environment with Traefik and PostgreSQL.
This repository provides a ready-to-run configuration to quickly launch a complete development environment for Smartstore locally using Docker Compose. 
Perfect for testing, developing, and debugging Smartstore applications.

---

## ✨ Features / Stack
* **Smartstore:** Uses the `nightly` image from `ghcr.io/smartstore/smartstore-linux`.
* **Database:** PostgreSQL `16`.
* **Reverse Proxy:** Traefik `v2.11` for easy routing.
* **HTTPS:** Local HTTPS for `localhost` via **automatically generated self-signed certificates by Traefik**.
* **API Ready:** Configuration for `Forwarded Headers` is enabled (`ASPNETCORE_FORWARDEDHEADERS_ENABLED=true`),
* allowing Smartstore to work correctly behind the proxy (important for API calls).
* **Traefik Dashboard:** Enabled for insights into routing (Default: `http://localhost:8080`).

---

## 📋 Prerequisites

* **Docker:** A recent version of Docker must be installed.
* Download: [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)
* **Docker Compose:**
    * Either as part of Docker Desktop (command: `docker compose`)
    * Or as a separate plugin/tool (Older command: `docker-compose`)
* **Git:** For cloning the repository.

---

## 🚀 Getting Started / Setup

1.  **Clone the repository**
2.  **Start containers**
    Start all services in the background:
    ```bash
    docker compose up -d
    ```
    On the first run, images will be downloaded and volumes created. This might take a moment.
    
---

## 💻 Usage / Accessing Services

After the containers have started (you can check the status with `docker compose ps`), you can access the services:

* **Smartstore Application:**
    * Open your browser and navigate to: `https://localhost`
    * **IMPORTANT:** Your browser will show a **security warning** because the SSL certificate used by Traefik is self-signed and not issued by a trusted Certificate Authority.
    * This is **expected and normal** for this local environment. Click on "Advanced" and "Proceed to localhost (unsafe)" (or similar) to continue.
    * On the first launch, it might take a little while for Smartstore to initialize.
    * **Setup Wizard:** Once Smartstore loads, you will likely encounter a setup wizard. When prompted for database details, use the following settings:
        * **Database System:** `PostgreSQL`
        * **Server Name:** `postgres` (This is the service name defined in `docker-compose.yml`)
        * **Database Name:** `mydatabase` 
        * **User:** `user`
        * **Password:** `password`

* **Traefik Dashboard:**
    * Open your browser and navigate to: `http://localhost:8080`
    * Here you can see the routers and services detected by Traefik.
    * **Note:** The dashboard is insecure by default (`--api.insecure=true`) and should only be used in trusted local environments.

* **PostgreSQL Database:**
    * Can be accessed via `localhost:5432` using a database tool (e.g., DBeaver, pgAdmin).
      
---

## ⚙️ Configuration

* The main configuration is done via `docker-compose.yml` file.
* PostgreSQL credentials are sourced from the `docker-compose.yml` file.
* The hostname for Smartstore is set to `localhost` in the Traefik labels within `docker-compose.yml` (`Host(\`localhost\`)`).

---

## ❓ Troubleshooting

* **Containers won't start:** Check the logs using `docker compose logs <service_name>` (e.g., `docker compose logs traefik`).
* **Port Conflicts:** If ports (80, 443, 5432, 8080) are already in use on your host machine, you need to adjust the port mappings in the `ports:` section of `docker-compose.yml` (e.g., change `- "80:80"` to `- "8081:80"`).
* **API Access / HTTPS Issues:** Ensure `ASPNETCORE_FORWARDEDHEADERS_ENABLED=true` is set in the `smartstore` service environment.
