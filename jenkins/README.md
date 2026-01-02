# Jenkins Shared Configurations

This folder contains shared Jenkins pipeline configurations for the microservices.

## Required Jenkins Credentials

Configure the following credentials in Jenkins:

| Credential ID | Type | Description |
|---------------|------|-------------|
| `dockerhub-credential` | Username/Password | Docker Hub credentials for pushing images |
| `aws-access-key-id` | Secret text | AWS Access Key ID |
| `aws-secret-access-key` | Secret text | AWS Secret Access Key |
| `aws-region` | Secret text | AWS Region (e.g., `us-east-1`) |
| `eks-cluster-name` | Secret text | EKS Cluster name |

## Pipeline Jobs

Each service has its own Jenkinsfile with stages:

1. **Dependencies** - Builds dependency libraries (e.g., `auth`, `account` interfaces)
2. **Build** - Runs `mvn clean package`
3. **Build & Push Image** - Pushes multi-arch Docker images to Docker Hub
4. **Deploy to EKS** - Updates Kubernetes deployment with new image

## Job Dependencies

```
auth (interface) ──┐
                   ├── auth-service
account (interface)┤
                   ├── account-service
                   └── gateway
```
