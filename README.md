# Composer Project

This folder contains the microservices architecture for the project, organized into services, shared libraries, and infrastructure configurations.

## Folder Structure

### Shared Libraries / Interfaces
These modules define the shared data objects (DTOs) and likely the HTTP clients (OpenFeign) for inter-service communication.

- **`account`**
  - Contains the interface definitions for the Account service.
  - **Objects**:
    - `AccountIn`:
      - `name` (String)
      - `email` (String)
      - `password` (String)
    - `AccountOut`:
      - `id` (String)
      - `name` (String)
      - `email` (String)

- **`auth`**
  - Contains the interface definitions for the Auth service.
  - **Objects**:
    - `LoginIn`:
      - `email` (String)
      - `password` (String)
    - `RegisterIn`:
      - `name` (String)
      - `email` (String)
      - `password` (String)
    - `TokenOut`:
      - `jwt` (String)

### Microservices
- **`account-service`**: Implementation of the Account service.
- **`auth-service`**: Implementation of the Auth service.
- **`gateway`**: The API Gateway (Spring Cloud Gateway) that acts as the entry point for the system.
- **`api`**: A custom **FastAPI** microservice (to be implemented manually). 
  - **Note**: Will use the utils library: [https://github.com/henriquebrnetto/utils](https://github.com/henriquebrnetto/utils)

### Infrastructure & DevOps

- **`jenkins`**: configuration files for the Jenkins CI/CD pipelines.
  - `Jenkinsfile.template`: A reusable pipeline logic for services.
    - **Stages**:
      1. **Dependencies**: Builds interface modules first (e.g., `auth`, `account`).
      2. **Build**: Maven build (`clean package`).
      3. **Build & Push Image**: Builds multi-platform Docker images (amy64, amd64) and pushes to DockerHub.
      4. **Deploy to EKS**: Updates the Kubernetes deployment on AWS EKS.

- **`k8s`**: Kubernetes manifests for deploying the services.
  - Contains configurations for:
    - Services: `account`, `auth`, `gateway`
    - Infrastructure: `db` (Postgres)
    - Configuration: `secrets`
  - **Deployment Order**: `secrets` -> `db` -> Services (`account`, `auth`, `gateway`).

## Gateway Configuration

The `gateway` service is configured to route traffic to the following internal services:

| Route ID | Path | URI |
| :--- | :--- | :--- |
| `exchange` | `/exchange/**` | `http://exchange:8080` |
| `account` | `/account/**` | `http://account:8080` |
| `auth` | `/auth/**` | `http://auth:8080` |
| `product` | `/product/**` | `http://product:8080` |
| `order` | `/order/**` | `http://order:8080` |

Global CORS configuration allows all origins (`*`), headers, and methods.

## DevOps Logic

1.  **Code Changes**: Pushed to the repository.
2.  **Jenkins Pipeline**:
    - Detects changes.
    - Builds shared libraries (`account`, `auth` generic modules) if necessary.
    - Builds the service artifact (Maven).
    - creating a Docker image using `docker buildx` for multi-architecture support.
    - Pushes the image to DockerHub (`henriquebrnetto/<service>`).
3.  **Deployment**:
    - Connects to AWS EKS.
    - Updates the deployment image to the new build ID.
    - Rolls out the update.