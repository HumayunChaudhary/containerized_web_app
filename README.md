# Containerized Web Application

A simple containerized web application demonstrating communication between a frontend and backend service using Docker networking.

The project consists of:

- A frontend served by Nginx
- A backend built with Node.js and Express
- Docker containers connected through a custom bridge network for internal DNS-based service discovery
- Frontend and backend containers communicate through the Docker host using `host.docker.internal`, configured via the `--add-host=host.docker.internal:host-gateway` flag on Linux

# Project Architecture

```text
                 ┌─────────────────────┐
                 │      Browser        │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │  Frontend Container │
                 │       (Nginx)       │
                 │      Port: 80       │
                 └──────────┬──────────┘
                            │
      proxy_pass http://host.docker.internal:3000
                            │
                            ▼
                 ┌─────────────────────┐
                 │  Backend Container  │
                 │ Node.js + Express   │
                 │      Port: 3000     │
                 └─────────────────────┘

                 Shared Docker Network:
                       app-network
```

---

# Tech Stack

## Frontend
- HTML
- CSS
- JavaScript
- Nginx

## Backend
- Node.js
- Express.js

## Containerization
- Docker
- Custom Docker bridge network

---

# Project Structure

```text
containerized_web_app/
│
├── frontend/
│   ├── index.html
│   ├── style.css
│   ├── script.js
│   ├── nginx.conf
│   ├── Dockerfile
│   └── .dockerignore
│
├── backend/
│   ├── index.js
│   ├── package.json
│   ├── Dockerfile
│   └── .dockerignore
```

---

# Application Behavior

The frontend communicates with the backend through Nginx reverse proxy configuration.

- If backend is running:
  
```text
Backend Status: OK
```

- If backend is unavailable:

```text
Backend Status: Error
```

---

# Important (Linux Users)  
 The flag below is required on Linux when a Docker container needs to access services running on the host machine using `host.docker.internal`.

 ```bash
 --add-host=host.docker.internal:host-gateway
 ```

 This creates a hostname mapping inside the container so that:

 ```text
 host.docker.internal
 ```

 resolves to the Docker host machine's IP address.

---

# Build Docker Images

## Build Backend Image

```bash
cd backend

docker build -t backend:latest .
```

## Build Frontend Image

```bash
cd ../frontend

docker build -t frontend:latest .
```

---

# Run Containers
## Run the frontend container:

 ```bash
 docker run \
   --add-host=host.docker.internal:host-gateway \
   -d \
   -p 8080:80 \
   --name frontend \
   frontend:latest
 ```

 Run the backend container:

 ```bash
 docker run \
   -d \
   -p 3000:3000 \
   --name backend \
   backend:latest
 ```
---

# Access the Application

Open your browser and visit:

```text
http://localhost
```

Or on an EC2 instance:

```text
http://<EC2-Public-IP>
```

---

# Verify Running Containers

```bash
docker ps
```

---

# Useful Docker Commands

## View Container Logs

```bash
docker logs frontend

docker logs backend
```

## Stop Containers

```bash
docker stop frontend backend
```

## Remove Containers

```bash
docker rm -f frontend backend
```

---

---

# Future Improvements

- Add Docker Compose support
- Deploy containers on AWS ECS
- Add CI/CD pipeline using Jenkins or GitHub Actions
- Add HTTPS support with Nginx

---

# Key Learning Outcomes

- Docker containerization
- Nginx reverse proxy configuration
- Node.js + Express backend integration
- Docker networking and service discovery
- Multi-container application architecture
- Internal DNS resolution in Docker

```
