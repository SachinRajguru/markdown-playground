# 📄 `02-multi-stage-multi-agent`

## Multi-Stage Multi-Agent Jenkins Pipeline

Study Guide + Practical Guide + Lab Guide + Technical Guide

### Table of Contents

1. [Introduction](#1-introduction)
2. [Objective](#2-objective)
3. [What is Multi-Stage Pipeline?](#3-what-is-multi-stage-pipeline)
4. [What is Multi-Agent Pipeline?](#4-what-is-multi-agent-pipeline)
5. [Why This Project Matters](#5-why-this-project-matters)
6. [Architecture Overview](#6-architecture-overview)
7. [Real-World 3-Tier Application Example](#7-real-world-3-tier-application-example)
8. [Project Structure](#8-project-structure)
9. [Jenkinsfile Walkthrough](#9-jenkinsfile-walkthrough)
10. [Understanding agent none](#10-understanding-agent-none)
11. [Backend Stage](#11-backend-stage)
12. [Frontend Stage](#12-frontend-stage)
13. [Database Stage](#13-database-stage)
14. [Pipeline Execution Flow](#14-pipeline-execution-flow)
15. [Dynamic Container Creation Demo](#15-dynamic-container-creation-demo)
16. [Why This Is Better Than VM Worker Nodes](#16-why-this-is-better-than-vm-worker-nodes)
17. [Real-World Use Cases](#17-real-world-use-cases)
18. [Practical Exercises](#18-practical-exercises)
19. [Troubleshooting](#19-troubleshooting)
20. [Interview Questions and Answers](#20-interview-questions-and-answers)
21. [Key Takeaways](#21-key-takeaways)

---

## 1. Introduction

In the previous project:

```text
01-my-first-pipeline
```

we ran:

* One stage
* One agent
* One container

That proved Docker agents work.

Now we extend that idea.

This project introduces:

* Multiple stages
* Multiple Docker agents
* Multiple containers
* Stage-specific execution environments

This is how real-world CI/CD pipelines are often built.

---

## 2. Objective

The goal is to simulate a 3-tier application using separate containers for:

* Frontend
* Backend
* Database

Each stage runs in its own dedicated Docker container.

That means:

* Frontend can use `Node.js` image
* Backend can use `Maven` image
* Database can use `MySQL` image

Each stage gets its own isolated runtime.

---

## 3. What is Multi-Stage Pipeline?

A multi-stage pipeline has multiple logical phases.

Example:

```text
Stage 1 → Backend Build
Stage 2 → Frontend Build
Stage 3 → Database Tasks
```

Instead of doing everything in one step:

We split work into stages.

This improves:

* Organization
* Debugging
* Scalability
* Maintainability

---

## 4. What is Multi-Agent Pipeline?

A multi-agent pipeline uses different execution agents.

Example:

```text
Frontend → Node Container
Backend → Maven Container
Database → MySQL Container
```

Different stages.

Different agents.

Different containers.

### Analogy

Think of building a house.

You do not use:

* One person for plumbing
* One person for electrical
* One person for roofing

Instead:

Each specialist handles their part.

That is exactly what multi-agent pipelines do.

---

## 5. Why This Project Matters

Real organizations rarely have:

* Single language apps
* Single-stage pipelines

They have:

* Java backend
* React frontend
* Database migrations
* Multiple toolchains

This project models that.

---

## 6. Architecture Overview

```text
Developer
 |
 v
Jenkins Master
 |
 v
Stage 1
Frontend
(Node Container)
 |
 v
Stage 2
Backend
(Maven Container)
 |
 v
Stage 3
Database
(MySQL Container)
 |
 v
Containers Destroyed
```

---

## 7. Real-World 3-Tier Application Example

Example application:

Frontend:

* React
* Angular
* Node.js

Backend:

* Java
* Spring Boot
* Maven

Database:

* MySQL
* PostgreSQL
* Oracle

Each layer can have independent CI/CD logic.

---

## 8. Project Structure

```text
02-multi-stage-multi-agent/
├── README.md
└── Jenkinsfile
```

---

## 9. Jenkinsfile Walkthrough

```groovy
pipeline {

    // No global agent
    // Each stage uses its own agent
    agent none

    stages {

        stage('Frontend') {

            // Frontend runs inside Node 20 container
            agent {
                docker {
                    image 'node:20-alpine'
                }
            }

            steps {

                // Verify Node is available
                sh 'node --version'

            }
        }

        stage('Backend') {

            // Backend runs inside Maven container
            agent {
                docker {
                    image 'maven:3.8'
                }
            }

            steps {

                // Verify Maven is available
                sh 'mvn --version'

            }
        }

        stage('Database') {

            // Database runs inside MySQL container
            agent {
                docker {
                    image 'mysql:8.4'
                }
            }

            steps {

                // Sample database command simulation
                sh 'echo select * from xyz;'

            }
        }

    }

}
```

---

## 10. Understanding agent none

```groovy
agent none
```

This is critical.

It means:

Do NOT run the entire pipeline using one global agent.

Instead:

Each stage defines its own agent.

Without:

```groovy
agent none
```

Jenkins may try using one execution environment.

That defeats multi-agent design.

---

## 11. Frontend Stage

Uses:

```text
node:16-alpine
```

Runs:

```bash
node --version
```

**Real-world**:

Replace with:

```bash
npm install
```

or

```bash
npm run build
```

---

## 12. Backend Stage

```groovy
stage('Backend')
```

Uses:

```groovy
maven:3.8
```

Runs:

```bash
mvn --version
```

**Real-world example**:

Instead of:

```bash
mvn --version
```

Use:

```bash
mvn clean install
```

or

```bash
mvn package
```

---

# 13. Database Stage

Uses:

```text
mysql:latest
```

Example:

```sql
select * from xyz;
```

**Real-world**:

Use:

* Schema migrations
* SQL migrations
* Liquibase
* Flyway

---

## 14. Pipeline Execution Flow

Execution:

```text
Stage 1
Create Node Container
Run frontend commands
Destroy container

Stage 2
Create Maven Container
Run backend commands
Destroy container

Stage 3
Create MySQL Container
Run DB commands
Destroy container
```

Each stage gets:

Fresh isolated container.

---

## 15. Dynamic Container Creation Demo

During execution:

Check:

```bash
docker ps
```

You may see:

```text
node container running
maven container running
```

After build:

```bash
docker ps
```

Expected:

```text
No running containers
```

Containers terminated automatically.

Exactly as shown in transcript.

---

## 16. Why This Is Better Than VM Worker Nodes

Traditional approach:

```text
Worker 1 → Node VM
Worker 2 → Maven VM
Worker 3 → DB VM
```

Problems:

* Maintain all VMs
* Upgrade packages
* Patch OS
* Dependency conflicts
* Security maintenance

Container approach:

Change:

```text
node:18-alpine
```

to

```text
node:20-alpine 
```

Done.

No VM maintenance.

Same for:

```text
maven:3.8
```

to

```text
maven:3.9
```

Done.

---

## 17. Real-World Use Cases

Used for:

* Microservices CI/CD
* Polyglot applications
* Multi-tier applications
* Enterprise delivery pipelines

Examples:

* React + Java + PostgreSQL
* Angular + Spring Boot + Oracle
* Node + Python + MongoDB

---

## 18. Practical Exercises

### Exercise 1

Replace:

```bash
node --version
```

with:

```bash
npm run build
```

### Exercise 2

Replace:

```bash
mvn --version
```

with:

```bash
mvn clean install
```

### Exercise 3

Add fourth stage:

```text
Security Scan
```

Use:

```text
sonarqube image
```

### Exercise 4

Replace:

```text
mysql:latest
```

with:

```text
postgres:15
```

---

## 19. Troubleshooting

### Error

```text
mvn command not found
```

Wrong image.

Check:

```text
maven:3.8
```

### Error

```text
node command not found
```

Verify:

```text
node:18-alpine
```

### Error

```text
docker permission denied
```

Fix:

```bash
sudo usermod -aG docker jenkins
sudo systemctl restart docker
```

### Error

Stage stuck.

Check:

* Docker daemon
* Jenkins restart
* Docker plugin

---

## 20. Interview Questions and Answers

**Q1 What is multi-stage pipeline?**

**Answer:**
```text
A Jenkins pipeline with multiple logical phases.
```

**Q2 What is multi-agent pipeline?**

**Answer:**
```text
Each stage runs on a different execution agent.
```

**Q3 What does agent none do?**

**Answer:**
```text
Disables global agent.

Allows stage-level agents.
```

**Q4 Why use separate images per stage?**

**Answer:**
```text
Provides:

• Dependency isolation
• Better scalability
• Easier maintenance
```

**Q5 Why containers over worker VMs?**

**Answer:**
```text
Containers:

• Lower cost
• Faster
• Easier upgrades
• Ephemeral
• Fewer conflicts
```

**Q6 How do you validate containers are created dynamically?**

**Answer:**

Use:

```bash
docker ps
```

During execution.

**Q7 Why is this useful for 3-tier apps?**

**Answer:**
```text
Frontend, backend and DB often require separate environments.

Multi-agent solves that.
```

---

## 21. Key Takeaways

This project introduced:

* agent none
* Multi-stage pipelines
* Multi-agent pipelines
* Stage-specific Docker images
* Dynamic container creation
* Ephemeral agent model

You now have a production-style pipeline structure.

---

## Next Project

Next:

```text
03-springboot-cicd/
```

Where we extend this into:

Full CI/CD:

* Build
* Test
* Docker Image
* Push
* Kubernetes Deployment
* ArgoCD GitOps

### Run This Project

```bash
git clone <repo>
cd 02-multi-stage-multi-agent
```

Configure Jenkins:

```text
Pipeline script from SCM
```

Use:

```text
02-multi-stage-multi-agent/Jenkinsfile
```

Run:

```text
Build Now
```

Watch:

```bash
docker ps
```

Observe containers appear dynamically.

Then disappear.

You have implemented a multi-stage multi-agent Jenkins pipeline.

---