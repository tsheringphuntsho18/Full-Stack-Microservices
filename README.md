# WEB303 Microservices & Serverless Applications

## Practical_03: Full-Stack Microservices with gRPC, Databases, and Service Discovery

This project demonstrates a production-ready microservices architecture implementation using Go, gRPC, Consul service discovery, and PostgreSQL databases, containerized with Docker for the WEB303 practical coursework.

## Overview

This practical demonstrates enterprise-grade microservices architecture patterns including:

- **Service-to-Service Communication**: High-performance gRPC protocol for internal communication
- **API Gateway Pattern**: Single entry point for external clients with HTTP REST interface
- **Service Discovery**: Dynamic service registration and discovery using HashiCorp Consul
- **Data Persistence**: Dedicated PostgreSQL databases per service following database-per-service pattern
- **Containerization**: Docker and Docker Compose for consistent deployment environments
- **Health Monitoring**: Integrated health checks and service monitoring


## Project Structure

```
/Full-Stack-Microservices
├── README.md
├── go.mod
├── go.sum
├── docker-compose.yml
│
├── api-gateway/
│   ├── Dockerfile
│   └── main.go
│
├── services/
│   ├── users-service/
│   │   ├── Dockerfile
│   │   └── main.go
│   │
│   └── products-service/
│       ├── Dockerfile
│       └── main.go
│
└── proto/
    ├── users.proto
    ├── products.proto
    └── gen/
        ├── users.pb.go
        ├── users_grpc.pb.go
        ├── products.pb.go
        └── products_grpc.pb.go
```

## Prerequisites

Before running this project, ensure you have the following installed:

- **Docker** (20.10+) and **Docker Compose** (2.0+)
- **Go** (1.19 or later) for local development
- **Protocol Buffers compiler** (`protoc`) for gRPC code generation
- **Git** for version control

### Verification Commands

```bash
docker --version
docker-compose --version
go version
protoc --version
```

## Setup Instructions

1. **Clone the repository:**

   ```bash
   git clone https://github.com/tsheringphuntsho18/Full-Stack-Microservices.git
   cd Full-Stack-Microservices
   ```

2. **Generate Protocol Buffer code (if modified):**

   ```bash
   # Install protoc-gen-go and protoc-gen-go-grpc
   go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
   go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest

   # Generate Go code from proto files
   protoc --go_out=. --go-grpc_out=. proto/*.proto
   ```

3. **Start all services with Docker Compose:**

   ```bash
   docker-compose up --build
   ```

4. **For development (running services locally):**

   ```bash
   # Install Go dependencies
   go mod tidy

   # Run individual services (in separate terminals)
   go run services/users-service/main.go
   go run services/products-service/main.go
   go run api-gateway/main.go
   ```

## Services and Ports

| Service          | Type       | Port  | Description                            |
| ---------------- | ---------- | ----- | -------------------------------------- |
| API Gateway      | HTTP REST  | 8080  | Main entry point for external requests |
| Users Service    | gRPC       | 50051 | User management microservice           |
| Products Service | gRPC       | 50052 | Product management microservice        |
| Consul           | HTTP       | 8500  | Service discovery and health checks    |
| Users DB         | PostgreSQL | 5432  | User data persistence                  |
| Products DB      | PostgreSQL | 55433 | Product data persistence               |

## Usage

- **API Gateway**: `http://localhost:8080`
- **Consul UI**: `http://localhost:8500` (for service discovery monitoring)
- **Health Checks**: Services register with Consul for health monitoring

## Development

### Project Structure Conventions

- Each microservice follows a clean architecture pattern
- Protocol Buffers define the service contracts
- Docker multi-stage builds optimize container sizes
- Health checks ensure service reliability

### Adding New Services

1. Define service contract in `proto/` directory
2. Generate Go code using `protoc`
3. Implement service logic in `services/` directory
4. Add service configuration to `docker-compose.yml`
5. Register service with Consul for discovery

## Troubleshooting

### Common Issues

**Port Conflicts**

```bash
# Check if ports are in use
netstat -tulpn | grep :8080
# Kill processes using the port if needed
sudo kill -9 $(lsof -t -i:8080)
```

**Docker Issues**

```bash
# Clean up containers and volumes
docker-compose down -v
docker system prune -f
```

**Service Discovery Issues**

```bash
# Check Consul logs
docker-compose logs consul
```

## License

This project is developed for educational purposes as part of WEB303 Microservices & Serverless Applications practical coursework.
