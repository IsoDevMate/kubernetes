# Kubernetes Application Documentation

This repository contains Kubernetes configurations for deploying a MongoDB database with a web application. The setup includes secrets management, deployments, and services.

## Repository Structure

```plaintext
├── mongo-secret.yml    # MongoDB credentials
├── configmap.yml       # MongoDB configuration
├── mongo.yaml          # MongoDB deployment and service
└── webapp.yml          # Web application deployment and service
<!--

Components Overview
1. Secret Configuration (mongo-secret.yml)
    - Contains encoded credentials for MongoDB authentication.
    - Base64 encoded username and password.

2. MongoDB Deployment (mongo.yaml)
    - Single replica deployment.
    - Windows Server Core-based MongoDB container.
    - Secret integration for authentication.
    - Internal service exposure.
    - Key features:
      - Image: mongo:windowsservercore-ltsc2022
      - Port: 27017 (MongoDB default)
      - Environment variables sourced from secrets.
      - ClusterIP service type for internal access.

3. Web Application Deployment (webapp.yml)
    - Single replica deployment.
    - Integration with MongoDB using ConfigMap and Secrets.
    - NodePort service for external access.
    - Key features:
      - Image: nanajanashia/k8s-demo-app:v1.0
      - Port: 3000 (application port)
      - NodePort: 30008 (external access)
      - Environment variables:
         - DB_URL (from ConfigMap)
         - USER_NAME (from Secret)
         - USER_PWD (from Secret)

Services
- MongoDB Service (mongo-service)
  - Type: ClusterIP
  - Port: 8080
  - Target Port: 27017

- Web Application Service (webapp-service)
  - Type: NodePort
  - Port: 8080
  - Target Port: 3000
  - Node Port: 30008

Deployment Instructions
1. Apply the secret first:
    ```bash
    kubectl apply -f mongo-secret.yml
    ```

2. Apply the ConfigMap:
    ```bash
    kubectl apply -f configmap.yml
    ```

3. Deploy MongoDB:
    ```bash
    kubectl apply -f mongo.yaml
    ```

4. Deploy the web application:
    ```bash
    kubectl apply -f webapp.yml
    ```

Security Notes
- Secrets are base64 encoded but should be handled securely.
- MongoDB credentials are managed through Kubernetes Secrets.
- Internal communication is handled through ClusterIP service.
- External access is only available to the web application through NodePort 30008.

Dependencies
- Kubernetes cluster
- kubectl CLI tool
- Access to specified container images:
  - mongo:windowsservercore-ltsc2022
  - nanajanashia/k8s-demo-app:v1.0

Network Architecture
- MongoDB runs on port 27017 (internal).
- Web application runs on port 3000 (internal).
- External access available on node port 30008.
-->
