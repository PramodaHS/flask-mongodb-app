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
