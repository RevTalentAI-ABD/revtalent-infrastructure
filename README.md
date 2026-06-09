# RevTalent Ecosystem Orchestration & Infrastructure

The **Infrastructure Repository** manages the orchestration configs, environment provisioning, and deployment setups for the entire **RevTalent** microservices project. It supports local developer workflows via Docker Compose and production scaling via Kubernetes.

---

## Local Development Orchestration (Docker Compose)

The repository provides a pre-configured `docker-compose.yml` file to quickly spin up all microservices and database engines.

### Third-Party Infrastructure Services Mapped:
1. **MySQL** (`mysql:8.0`): Relational store on port `3307`.
2. **MongoDB** (`mongo:7.0`): Document store on port `27017`.
3. **RabbitMQ** (`rabbitmq:3-management`): AMQP broker on port `5672` (Console at `15672`).
4. **ChromaDB** (`chromadb/chroma`): Vector DB on port `8000`.
5. **Ollama** (`ollama/ollama`): Local LLM inference engine on port `11434`.

### App Services Orchestrated:
- **eureka-server** (Port: `8762`)
- **config-server** (Port: `8888`)
- **api-gateway** (Port: `8090`)
- **auth-service** (Port: `8091`)
- **employee-service** (Port: `8092`)
- **leave-service** (Port: `8093`)
- **payroll-service** (Port: `8094`)
- **performance-service** (Port: `8095`)
- **recruitment-service** (Port: `8096`)
- **ai-service** (Port: `8097`)
- **frontend** (Port: `5173`)

### Commands to Run Locally:
```bash
# Start all databases and third-party containers
docker-compose up -d mysql mongodb rabbitmq chromadb ollama

# Start the complete microservice application stack
docker-compose up -d
```

---

## Production Deployments (Kubernetes Manifests)

The `k8s/` folder contains structured Kubernetes manifests to provision and scale the environment (e.g. in Azure Kubernetes Services - AKS):

### 1. Database & Middleware Manifests (`k8s/infrastructure/`)
Contains deployment definitions, cluster IPs, and config maps:
- `mysql.yaml`
- `mongodb.yaml`
- `rabbitmq.yaml`
- `chromadb.yaml`
- `ollama.yaml`

### 2. Application Services Manifests (`k8s/services/`)
Specifies deployment parameters, replica counts, cluster routing services, and security variables:
- `eureka-server.yaml`
- `config-server.yaml`
- `api-gateway.yaml`
- `auth-service.yaml`
- `employee-service.yaml`
- `leave-service.yaml`
- `payroll-service.yaml`
- `performance-service.yaml`
- `recruitment-service.yaml`
- `ai-service.yaml`
- `frontend.yaml`
- `revtalent-secrets.yaml` (Secret keys & DB credentials)

### Commands to Apply to Kubernetes Cluster:
```bash
# Create target production namespace (if needed)
kubectl create namespace revtalent-prod

# Apply infrastructure services
kubectl apply -f k8s/infrastructure/ -n revtalent-prod

# Apply microservices and secrets configurations
kubectl apply -f k8s/services/ -n revtalent-prod
```
