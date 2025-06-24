# DevOps Internship Assignment – NGINX Reverse Proxy + Docker Compose

This project sets up two backend services (Go and Python Flask) behind an NGINX reverse proxy using Docker Compose. All services are accessible via a single port using path-based routing.

---

## Project Structure

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

---

## Services

### Service 1 – Golang

- Runs on port 8001 inside container  
- Endpoints:
  - GET /service1/ping → {"status": "ok", "service": "1"}
  - GET /service1/hello → {"message": "Hello from Service 1"}

### Service 2 – Python

- Runs on port 8002 using `uv run app.py`  
- Endpoints:
  - GET /service2/ping → {"status": "ok", "service": "2"}
  - GET /service2/hello → {"message": "Hello from Service 2"}

### NGINX – Reverse Proxy

- Accepts all traffic at http://<EC2-IP>:8080  
- Routes:
  - /service1 → Service 1
  - /service2 → Service 2
- Logs request path and timestamp  
- Performs basic health checks on both services  

---

## How to Run

Ensure Docker and Docker Compose are installed. Then run: docker-compose up --build


This builds and launches all services in containers using bridge networking.

---

## Requirements Met

- NGINX reverse proxy (containerized)
- Path-based routing using `/service1` and `/service2`
- All services accessible via a single port (`8080`)
- Request logging in NGINX
- Health check endpoints (`/ping`)
- One-command system startup

---

## Health Check

Use curl or browser to verify services:

curl http://<EC2-IP>:8080/service1/ping
curl http://<EC2-IP>:8080/service2/hello

Expect valid JSON responses with status or message.

---

## Challenges Faced

- Go module (`go mod`) compatibility required base image version update
- Virtual environment setup for Python required fixing `.venv` inside container
- NGINX health check failed initially due to missing curl in Go container
- Fixed NGINX routing by careful `location /serviceX` block config
- Handled Docker container conflicts using `docker container prune(not recommended by me)`

---

## EC2 Notes

- Ubuntu EC2 instance used for deployment  
- Ports 8080, 8001, 8002 opened in security group  
- Docker and Docker Compose installed  
- Services run using isolated bridge networking  

---
## Loom Video

The demo video covers:

- Initial EC2 setup and dependency installation  
- Service 1 and 2 testing manually  
- Dockerfile creation for each  
- NGINX reverse proxy logic and container setup  
- Docker Compose integration  
- Final testing and health check demonstration  

---

## Author

Vaishnavi Golhar  
DevOps & Cloud Enthusiast


