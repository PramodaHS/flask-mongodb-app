# Flask and MongoDB on Kubernetes

This project demonstrates how to deploy a Python Flask application that interacts with a MongoDB database on a Kubernetes cluster. The deployment includes setting up services, volumes, autoscaling, database authentication, and resource management.

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Setup Instructions](#setup-instructions)
  - [1. Build and Push Docker Image](#1-build-and-push-docker-image)
  - [2. Deploy to Kubernetes](#2-deploy-to-kubernetes)
  - [3. Verify Deployment](#3-verify-deployment)
  - [4. Access the Application](#4-access-the-application)
- [DNS Resolution in Kubernetes](#dns-resolution-in-kubernetes)
- [Resource Requests and Limits](#resource-requests-and-limits)
- [Design Choices](#design-choices)
- [Testing Scenarios](#testing-scenarios)
- [Conclusion](#conclusion)

## Overview
 The setup of a Python Flask application and to configure it to connect to MongoDB, then Deploy the Python Flask application that interacts with a MongoDB database on a Kubernetes cluster, ensuring proper setup of services, volumes, autoscaling, database authentication, DNS resolution, and resource management.

## Prerequisites

- **Minikube or Docker Desktop with Kubernetes enabled**
- **kubectl**: Command-line tool for interacting with Kubernetes clusters.
- **Docker**: For building and pushing Docker images.

## Setup Instructions

### 1. Build and Push Docker Image

1. **Build the Docker Image:**
   ```bash
   docker build -t flask-mongo-app:latest .

2. **Tag the Docker Image:**
   For Docker Desktop’s local registry:
   ```bash
   docker tag flask-mongo-app:latest localhost:5000/flask-mongo-app:latest
3. **Push the Docker Image:**
   Push the image to the local registry:
   ```bash
   docker push localhost:5000/flask-mongo-app:latest
### 2. Deploy to Kubernetes
  Apply Kubernetes Resources:
  Deploy the Flask application and MongoDB using the following commands:
  ```bash
  kubectl apply -f k8s/flask-deployment.yaml
  kubectl apply -f k8s/flask-service.yaml
  kubectl apply -f k8s/mongo-statefulset.yaml
  kubectl apply -f k8s/mongo-service.yaml
  kubectl apply -f k8s/flask-hpa.yaml
  ```
### 3. Verify Deployment
   Check the status of all resources:
   ```bash
   kubectl get all
   Ensure that all pods are running and the services are configured correctly.
   ```

  Check HPA (Horizontal Pod Autoscaler):
  ```bash
  Verify that the HPA is configured and working:
  kubectl get hpa
  ```

### 4. Access the Application
  Access the Flask Application:
  The Flask service should be accessible at http://localhost:8080/. You can verify it using:
      ```bash
      curl http://localhost:8080/
      ```
  You should receive a response like:
  ```text
  Welcome to the Flask app! The current time is: <Date and Time>
  Interact with the /data Endpoint:
  ```

Insert Data:
```bash
curl -X POST -H "Content-Type: application/json" -d '{"key":"value"}' http://localhost:8080/data
```
Retrieve Data:
```bash
curl http://localhost:8080/data
```

# DNS Resolution in Kubernetes
Kubernetes uses CoreDNS for DNS resolution within the cluster. Each service in Kubernetes is assigned a DNS name, which can be used by pods to communicate with each other. For example, the Flask application can connect to MongoDB using the DNS name mongo-service.default.svc.cluster.local. This setup allows for seamless inter-pod communication without needing to hardcode IP addresses.

# Resource Requests and Limits
In Kubernetes, resource requests and limits are used to manage CPU and memory usage:

Resource Requests: These define the minimum amount of resources that a container is guaranteed. Kubernetes uses these values when scheduling containers.
Resource Limits: These define the maximum amount of resources that a container can use. If the container exceeds these limits, it may be throttled or terminated.
Example configuration in flask-deployment.yaml:

```yaml
resources:
  requests:
    memory: "256Mi"
    cpu: "0.2"
  limits:
    memory: "512Mi"
    cpu: "0.5"
```

# Design Choices
Container Registry: Docker Desktop’s local registry was chosen for simplicity and speed during the development phase. An alternative could be Docker Hub or any cloud container registry, but using the local registry allows faster iterations.
MongoDB StatefulSet: StatefulSet was used to ensure that MongoDB’s data persists across pod restarts, maintaining a consistent identity for each MongoDB pod.
Autoscaling: Horizontal Pod Autoscaler (HPA) was configured to manage traffic spikes, ensuring that the application scales up to handle increased load and scales down to save resources during low traffic periods.

# Testing Scenarios
1. Autoscaling Test:
Method: Simulated high CPU load using a stress testing tool and observed how the HPA scaled the number of Flask pods.

Outcome: The application successfully scaled up when CPU usage exceeded the defined threshold.

3. Database Interaction Test:
Method: Tested the /data endpoint by inserting and retrieving data, ensuring MongoDB was functioning correctly with authentication.

Outcome: Data was inserted and retrieved successfully, confirming proper communication between the Flask app and MongoDB.

**Issues Encountered:**
During testing, an internal server error was encountered when accessing the /data endpoint, which was resolved by verifying the database connection details and ensuring MongoDB authentication was correctly configured.
# Conclusion
This project demonstrates a full-stack deployment on Kubernetes, including the use of Docker for containerization, Kubernetes for orchestration, and MongoDB as the database. The setup is scalable, secure, and ready for further development or production deployment.
