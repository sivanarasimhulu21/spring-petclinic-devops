
# Spring Petclinic – DevOps Deployment Project

This project is based on the official **Spring Petclinic** application.

The main purpose of this repository is to practice and demonstrate how a Java Spring Boot application can be containerized, continuously integrated, deployed on Kubernetes, and monitored using modern DevOps tools.

I used the Spring Petclinic application as the base application and implemented the **DevOps workflow, deployment, containerization, CI/CD, Kubernetes configuration, and monitoring**.

---

# What is Spring Petclinic?

Spring Petclinic is a sample Java Spring Boot web application used to demonstrate a typical web application.

It provides a simple veterinary clinic management system where users can manage:

* Pet owners
* Veterinarians
* Pets
* Visits
* Pet types
* Veterinarian specialties

The application provides a practical example of a Spring Boot application that can be used for learning application development as well as DevOps deployment.

---

# What I Did in This Project

I used the Spring Petclinic application to build a complete DevOps deployment workflow.

The main work performed in this project includes:

* Set up the Spring Boot application locally
* Created a Docker image for the application
* Ran the application using Docker
* Created a Jenkins CI/CD pipeline
* Automated application build using Maven
* Integrated GitHub with Jenkins
* Created Kubernetes deployment configuration
* Deployed the application to Kubernetes
* Exposed the application using Kubernetes Service
* Configured Prometheus for monitoring
* Configured Grafana for visualization
* Monitored application and system metrics
* Troubleshot containers, deployments, services, and monitoring components

---

# Tools and Technologies Used

| Tool / Technology | Purpose                           |
| ----------------- | --------------------------------- |
| Java              | Application development           |
| Spring Boot       | Application framework             |
| Maven             | Build and package application     |
| Git               | Version control                   |
| GitHub            | Source code repository            |
| Docker            | Application containerization      |
| Jenkins           | CI/CD automation                  |
| Kubernetes        | Container orchestration           |
| Prometheus        | Metrics collection and monitoring |
| Grafana           | Metrics visualization             |
| Linux             | DevOps environment                |
| AWS EC2           | Cloud infrastructure              |

---

# DevOps Architecture

```text
                         GitHub
                           |
                           |
                           v
                       Jenkins
                    CI/CD Pipeline
                           |
             +-------------+-------------+
             |                           |
          Maven Build              Docker Build
             |                           |
             |                           v
             |                    Docker Image
             |                           |
             +-------------+-------------+
                           |
                           v
                      Kubernetes
                           |
                +----------+----------+
                |                     |
                v                     v
          Petclinic Pod          Kubernetes Service
                |                     |
                +----------+----------+
                           |
                           v
                    Spring Petclinic
                           |
                           v
                     Application
                           
                           |
                           v
                     Prometheus
                           |
                           v
                       Grafana
```

---

# 1. Application Setup

The project starts with the Spring Petclinic Spring Boot application.

The application is built using Maven.

Basic build command:

```bash
./mvnw clean package
```

The generated JAR file can be used to run the application.

```bash
java -jar target/*.jar
```

---

# 2. Docker

Docker is used to package the Spring Petclinic application together with the required runtime environment.

## Docker Workflow

```text
Spring Petclinic Source Code
          |
          v
       Dockerfile
          |
          v
     Docker Image
          |
          v
    Docker Container
          |
          v
 Spring Petclinic App
```

## Build Docker Image

```bash
docker build -t spring-petclinic .
```

## Run Docker Container

```bash
docker run -d \
  --name spring-petclinic \
  -p 8080:8080 \
  spring-petclinic
```

Check running containers:

```bash
docker ps
```

View application logs:

```bash
docker logs spring-petclinic
```

Stop the container:

```bash
docker stop spring-petclinic
```

---

# 3. Jenkins CI/CD

Jenkins is used to automate the CI/CD process.

Instead of manually building and deploying the application every time, Jenkins performs the required steps through a pipeline.

## Jenkins Pipeline

```text
Developer
    |
    v
GitHub
    |
    v
Jenkins
    |
    +---- Checkout
    |
    +---- Maven Build
    |
    +---- Test
    |
    +---- Docker Build
    |
    +---- Docker Image
    |
    v
Kubernetes Deployment
```

## Pipeline Stages

The Jenkins pipeline can contain stages such as:

```text
1. Checkout
2. Build
3. Test
4. Docker Build
5. Docker Push
6. Deploy
7. Verify
```

The pipeline is defined using a `Jenkinsfile`.

Example:

```groovy
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh './mvnw clean package'
            }
        }

        stage('Test') {
            steps {
                sh './mvnw test'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t spring-petclinic .'
            }
        }

        stage('Deploy') {
            steps {
                sh 'kubectl apply -f k8s/'
            }
        }
    }
}
```

---

# 4. Kubernetes

Kubernetes is used to deploy and manage the containerized Spring Petclinic application.

Instead of manually running Docker containers, Kubernetes manages the application containers.

## Kubernetes Architecture

```text
                 Kubernetes Cluster
                        |
             +----------+----------+
             |                     |
             v                     v
        Petclinic Pod          Monitoring
             |
             v
      Spring Petclinic
             |
             v
       Kubernetes Service
             |
             v
          User
```

## Kubernetes Components Used

### Deployment

The Deployment manages the Spring Petclinic Pods.

```yaml
apiVersion: apps/v1
kind: Deployment
```

It can be used to:

* Create Pods
* Maintain desired replicas
* Restart failed Pods
* Perform rolling updates

### Service

The Kubernetes Service provides network access to the application.

```yaml
apiVersion: v1
kind: Service
```

---

# Kubernetes Commands

Apply the deployment:

```bash
kubectl apply -f k8s/
```

Check Pods:

```bash
kubectl get pods
```

Check Deployments:

```bash
kubectl get deployments
```

Check Services:

```bash
kubectl get svc
```

Check application logs:

```bash
kubectl logs <pod-name>
```

Describe a Pod:

```bash
kubectl describe pod <pod-name>
```

---

# 5. Prometheus

Prometheus is used for **metrics collection and monitoring**.

It collects metrics from the application and/or Kubernetes environment and stores them as time-series data.

The monitoring flow is:

```text
Application / Kubernetes
          |
          v
      Prometheus
          |
          v
     Metrics Data
```

Prometheus can be used to monitor information such as:

* CPU usage
* Memory usage
* Application metrics
* Request-related metrics
* Container metrics
* Kubernetes metrics

---

# 6. Grafana

Grafana is used to visualize the metrics collected by Prometheus.

The monitoring architecture is:

```text
Application
     |
     v
 Prometheus
     |
     | Metrics
     v
  Grafana
     |
     v
 Dashboards
```

Grafana dashboards make it easier to understand the health and performance of the application and infrastructure.

Examples of information that can be visualized:

* CPU utilization
* Memory utilization
* Pod status
* Container metrics
* Application performance
* Kubernetes resource usage

---

# Complete DevOps Workflow

The complete workflow implemented in this project is:

```text
                    Developer
                        |
                        v
                    GitHub
                        |
                        v
                    Jenkins
                        |
             +----------+----------+
             |          |          |
             v          v          v
          Checkout    Maven      Tests
                        |
                        v
                  Docker Build
                        |
                        v
                   Docker Image
                        |
                        v
                  Kubernetes
                        |
                +-------+-------+
                |               |
                v               v
             Pod(s)          Service
                |               |
                +-------+-------+
                        |
                        v
                 Petclinic App
                        |
                        v
                   Prometheus
                        |
                        v
                    Grafana
                        |
                        v
                   Monitoring
```

---

# Project Structure

```text
spring-petclinic/
│
├── src/
├── pom.xml
├── Dockerfile
├── Jenkinsfile
│
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
│
└── README.md
```

The exact files may change as the project evolves.

---

# How to Run the Project

## Run Locally

```bash
./mvnw spring-boot:run
```

Or:

```bash
./mvnw clean package
java -jar target/*.jar
```

---

## Run Using Docker

Build the image:

```bash
docker build -t spring-petclinic .
```

Run the container:

```bash
docker run -d \
  --name spring-petclinic \
  -p 8080:8080 \
  spring-petclinic
```

Open:

```text
http://localhost:8080
```

---

## Deploy to Kubernetes

Apply Kubernetes configuration:

```bash
kubectl apply -f k8s/
```

Check the deployment:

```bash
kubectl get deployments
```

Check Pods:

```bash
kubectl get pods
```

Check Service:

```bash
kubectl get svc
```

---

# Monitoring

After deploying the application, Prometheus is used to collect metrics and Grafana is used to create monitoring dashboards.

```text
Kubernetes
     |
     +---- Pods
     +---- Containers
     +---- Application
              |
              v
         Prometheus
              |
              v
           Grafana
              |
              v
        Monitoring Dashboard
```

---

# What I Learned

This project helped me understand how different DevOps tools work together in a real deployment workflow.

### Application

* Spring Boot
* Maven
* Java

### Containerization

* Docker
* Dockerfile
* Docker containers
* Docker images

### CI/CD

* Jenkins
* Jenkinsfile
* Pipeline stages
* GitHub integration
* Automated builds

### Orchestration

* Kubernetes
* Pods
* Deployments
* Services
* Application deployment

### Monitoring

* Prometheus
* Grafana
* Metrics
* Monitoring dashboards

### Infrastructure / Linux

* Linux commands
* Process management
* Container troubleshooting
* Application logs
* Kubernetes troubleshooting

---

# Key DevOps Concepts Demonstrated

This project demonstrates a complete DevOps workflow:

```text
Source Control
      ↓
CI/CD
      ↓
Build
      ↓
Containerization
      ↓
Orchestration
      ↓
Deployment
      ↓
Monitoring
```

The project is useful for understanding how a real application moves from **source code to automated deployment and monitoring**.

---

# Disclaimer

The Spring Petclinic application is based on the official Spring Petclinic project.

The application code is used as a practical base for implementing and learning DevOps practices. The DevOps configurations, deployment process, CI/CD pipeline, containerization, Kubernetes setup, and monitoring implementation in this repository are my hands-on work and learning.

---

# Author

**Siva Narasimhulu**

DevOps | Cloud | Linux | Kubernetes

---

⭐ If you find this project useful, feel free to star the repository.
