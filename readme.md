# DevOps Internship Assignment – NGINX Reverse Proxy + Docker Compose

This project demonstrates a production-style setup where two backend services—one in Go and one in Python (Flask)—are deployed using Docker Compose and routed via a centralized NGINX reverse proxy. All services are accessible through a single port using path-based routing (`/service1`, `/service2`).

---

## Project Structure

```
.
├── docker-compose.yml
├── nginx/
│   ├── nginx.conf
│   └── Dockerfile
├── service_1/
│   ├── main.go
│   ├── go.mod
│   └── Dockerfile
├── service_2/
│   ├── app.py
│   ├── pyproject.toml
│   ├── Dockerfile
│   └── .venv (excluded)
└── README.md
```

---

## Services Overview

### Service 1 – Go (Golang)

* Runs on port `8001` inside its container
* Endpoints:

  * `GET /service1/ping` → `{"status": "ok", "service": "1"}`
  * `GET /service1/hello` → `{"message": "Hello from Service 1"}`

### Service 2 – Python (Flask)

* Runs on port `8002` using `uv run app.py`
* Endpoints:

  * `GET /service2/ping` → `{"status": "ok", "service": "2"}`
  * `GET /service2/hello` → `{"message": "Hello from Service 2"}`

### NGINX – Reverse Proxy

* Exposes the unified entrypoint: `http://<EC2-IP>:8080`
* Routes requests to:

  * `/service1/*` → Service 1
  * `/service2/*` → Service 2
* Logs each request with timestamp and path
* Performs health checks on both services before starting

---

## How to Run

Ensure Docker and Docker Compose are installed. Then run:

```bash
docker-compose up --build
```

This will:

* Build Docker images for all services
* Launch containers with bridge networking
* Wait for health checks before starting NGINX

---

## Requirements Met

* Path-based routing via `/service1` and `/service2`
* NGINX in a container, not on host
* Single port access (8080) for both services
* Request logging in NGINX (timestamp + path)
* Health checks with `/ping` endpoints
* One-command startup using Docker Compose

---

## Health Check Testing

You can verify services using:

```bash
curl http://<EC2-IP>:8080/service1/ping
curl http://<EC2-IP>:8080/service2/hello
```

Or open them in your browser. You should see valid JSON responses.

---

## Challenges Faced

* Go container required an updated base image for `go.mod` compatibility
* Python's `.venv` setup broke builds until `uv` was properly integrated
* Missing `curl` in Go container caused initial health check failure
* NGINX reverse proxy didn't work until precise location path handling was done
* Docker container name conflicts required pruning old containers with caution

---

## EC2 Hosting Notes

* Deployed on Ubuntu EC2 Instance
* Opened ports: `8080`, `8001`, `8002` in security group
* Installed Docker + Docker Compose
* Services run inside isolated Docker bridge network

---

## Loom Video (Demo Script Highlights)

The video includes:

* Full EC2 setup (security groups, SSH)
* Testing Go and Python apps independently
* Dockerfile creation and testing
* NGINX reverse proxy configuration
* Docker Compose integration and troubleshooting
* Final verification via browser and curl

---

## Author

Vaishnavi Golhar
DevOps & Cloud Enthusiast
