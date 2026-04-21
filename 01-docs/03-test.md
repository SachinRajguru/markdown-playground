# 📄 `01-my-first-pipeline`

## Jenkins First Pipeline

Study Guide + Practical Guide + Lab Guide + Technical Guide

### Table of Contents

1. [Introduction](#1-introduction)
2. [Objective of This Project](#2-objective-of-this-project)
3. [Project Architecture](#3-project-architecture)
4. [Why This Pipeline Matters](#4-why-this-pipeline-matters)
5. [Prerequisites](#5-prerequisites)
6. [Project Structure](#6-project-structure)
7. [Jenkinsfile Walkthrough](#7-jenkinsfile-walkthrough)
8. [Understanding the Pipeline Components](#8-understanding-the-pipeline-components)
9. [Lab Setup Steps](#9-lab-setup-steps)
10. [Execute the Pipeline](#10-execute-the-pipeline)
11. [Expected Build Flow](#11-expected-build-flow)
12. [Validate Docker Agent Behavior](#12-validate-docker-agent-behavior)
13. [How Container Lifecycle Works](#13-how-container-lifecycle-works)
14. [Why Docker Agents Are Better Than Static Worker Nodes](#14-why-docker-agents-are-better-than-static-worker-nodes)
15. [Real-World Use Cases](#15-real-world-use-cases)
16. [Common Issues and Troubleshooting](#16-common-issues-and-troubleshooting)
17. [Practical Exercises](#17-practical-exercises)
18. [Interview Questions and Answers](#18-interview-questions-and-answers)
19. [Key Takeaways](#19-key-takeaways)
20. [Next Project](#20-next-project)

---

## 1. Introduction

This project is the **first practical Jenkins pipeline** used to validate whether:

* Jenkins is installed correctly
* Docker is configured as an agent
* Docker Pipeline plugin works
* Jenkins can pull a container image
* Jenkins can run a pipeline inside a container
* Jenkins can automatically destroy the container after execution

This is not intended to be a full CI/CD pipeline.

It is a **verification pipeline**.

Think of it as a “Hello World” for `Jenkins + Docker agents`.

---

## 2. Objective of This Project

The goal is simple:

Run a Jenkins pipeline inside a Docker container and verify the configuration works.

This pipeline uses:

* Jenkins
* Docker Agent
* Node.js Container

The pipeline performs one test:

```bash
node --version
```

If Node version is returned successfully:

* Docker agent works
* Jenkins can run jobs inside containers
* Docker permissions are correct
* Plugin configuration is correct

---

## 3. Project Architecture

### Flow

```text
Developer
   |
   |
   v
Jenkins Job Triggered
   |
   v
Jenkins Master Schedules Job
   |
   v
Docker Plugin Requests Container
   |
   v
Node.js Container Starts
   |
   v
Pipeline Executes
(node --version)
   |
   v
Job Completes
   |
   v
Container Deleted Automatically
```

### Analogy

Imagine booking a taxi:

Traditional worker node:

* Buy a permanent taxi.
* Maintain it.
* Pay for it all month.
* Use it occasionally.

Docker Agent:

* Book Uber only when needed.
* Ride.
* Finish.
* Uber disappears.

That is exactly how Docker agents behave.

---

## 4. Why This Pipeline Matters

Before building:

* Multi-stage pipelines
* CI/CD pipelines
* Kubernetes deployment pipelines

You first verify:

* Jenkins works
* Docker works
* Agent configuration works

This project is that verification step.

---

## 5. Prerequisites

You should have:

### Infrastructure

* Ubuntu EC2 Instance
* Jenkins installed
* Docker installed
* Port 8080 open

### Jenkins Configuration

Installed:

* Docker Pipeline Plugin

Configured:

```bash
sudo usermod -aG docker jenkins
sudo systemctl restart docker
```

## Validate Docker Access

Switch to Jenkins user:

```bash
su - jenkins
```

Test:

```bash
docker run hello-world
```

If this works:

You are ready.

---

## 6. Project Structure

```text
01-my-first-pipeline/
├── README.md
└── Jenkinsfile
```

---

## 7. Jenkinsfile Walkthrough

### Jenkinsfile

```groovy
pipeline {

    // Defines where this pipeline will run
    agent {

        // Use a Docker-based ephemeral agent
        docker {

            // Pull Node 20 lightweight Alpine image
            image 'node:20-alpine'
        }
    }

    // Defines pipeline stages
    stages {

        // Single test stage
        stage('Test') {

            steps {

                // Run command inside container
                // Verifies Node is installed and agent works
                sh 'node --version'

            }
        }
    }

}
```

---

## 8. Understanding the Pipeline Components

### `pipeline {}`

Defines entire Jenkins pipeline.

Think of this as the skeleton.

### `agent`

```groovy
agent {
 docker {
   image 'node:20-alpine'
 }
}
```

This tells Jenkins:

Do not run on Jenkins host.

Create Docker container.

**Use:**

```text
node:20-alpine
```

### Why node:20-alpine?

* Small image
* Lightweight
* Fast pull
* Includes Node installed

### `stages`

```groovy
stages {

}
```

Stages = logical phases.

This pipeline has only one.

### `stage`

```groovy
stage('Test')
```

Test stage.

**Purpose:**

Verify Node exists inside container.

### `steps`

```groovy
steps {
 sh 'node --version'
}
```

Commands executed.

---

## 9. Lab Setup Steps

### Step 1

- Go to Jenkins:

```text
http://your-server:8080
```

### Step 2

- Create New Item

### Step 3

- Select:

```text
Pipeline
```

- Name:

```text
first-jenkins-job
```

### Step 4

- Choose:

```text
Pipeline script from SCM
```

### Step 5

- Repository:

```text
https://github.com/yourrepo/jenkins-guide
```

- Branch:

```text
main
```

- Script Path:

```text
01-my-first-pipeline/Jenkinsfile
```

- **Save**.

---

## 10. Execute Pipeline

- Click:

```text
Build Now
```

---

## 11. Expected Build Flow

Jenkins console output should show:

* Checkout from GitHub
* Pull node:20-alpine
* Create Docker container
* Run:

```bash
node --version
```

Example:

```text
v20.x.x
```

Then:

```text
Finished: SUCCESS
```

---

## 12. Validate Docker Agent Behavior

SSH into server.

- Check:

```bash
docker ps
```

Expected:

```text
No containers running
```

- Check:

```bash
docker ps -a
```

You should not see pipeline container.

Container was destroyed.

That is expected.

---

## 13. How Container Lifecycle Works

### During Job

Container exists.

```text
Container Created
Pipeline Runs
Container Deleted
```

### Temporary (Ephemeral) Agent

This is called:

Ephemeral Agent.

Created only when needed.

Destroyed after use.

---

## 14. Why Docker Agents Are Better Than Worker Nodes

### Traditional Worker Node Problem

Suppose worker node has:

* Node 18
* Python 3.9
* Java 11

Another team wants:

* Node 20
* Python 3.12
* Java 17

Conflict.

### Docker Solves This

Today:

```text
node:18-alpine
```

Tomorrow:

```text
node:20-alpine
```

Change:

```text
Two characters.
```

Done.

No server maintenance.

---

## 15. Real-World Use Cases

This same pattern is used for:

* Node builds
* NPM pipelines
* React builds
* Angular builds
* Frontend test pipelines

Instead of:

```bash
node --version
```

Use:

```bash
npm install
```

or

```bash
npm run build
```

Same structure.

---

## 16. Common Issues and Troubleshooting

### Error

```text
permission denied docker.sock
```

Fix:

```bash
sudo usermod -aG docker jenkins
sudo systemctl restart docker
```

Restart Jenkins.

### Error

```text
docker not found
```

Install Docker.

```bash
sudo apt install docker.io -y
```

### Error

```text
Cannot pull image
```

Check:

* Internet
* Docker daemon
* Image name

---

## 17. Practical Exercises

### Exercise 1

Change:

```text
node:20-alpine
```

to

```text
node:22-alpine
```

Run again.

### Exercise 2

Replace:

```bash
node --version
```

with:

```bash
npm --version
```

### Exercise 3

Run:

```bash
npm install
```

using sample Node application.

---

## 18. Interview Questions and Answers

**Q1 What is Docker agent in Jenkins?**

**Answer:**
```text
Docker agent allows Jenkins to run pipelines inside temporary containers instead of static worker nodes.
```

**Q2 Why use Docker agents?**

**Answer:**
```text
Benefits:

• Dependency isolation
• No package conflicts
• Lower cost
• Ephemeral containers
• Easier upgrades
```

**Q3 Difference between Jenkins worker and Docker agent?**

**Answer:**
```text
Worker Node:

• Long-running machine
• Requires maintenance
• Persistent

Docker Agent:

• Temporary container
• Created on demand
• Destroyed after execution
```

**Q4 Why use node:20-alpine?**

**Answer:**
```text
Lightweight Node image for fast builds.
```

**Q5. What does `docker ps` verify?**

**Answer:**
```text
Confirms whether pipeline containers are running.

In this case:

No running container proves auto-cleanup.
```

**Q6. What does this pipeline validate?**

**Answer:**
```text
It validates:

• Jenkins
• Docker
• Plugin
• Permissions
• Agent configuration
```

---

## 19. Key Takeaways

This project proves:

* Jenkins can run pipelines
* Docker can act as Jenkins agent
* Containers can be created dynamically
* Containers can be destroyed automatically
* Docker agents are better than static worker nodes

This is the foundation for:

* Multi-stage pipelines
* Multi-agent pipelines
* Full CI/CD
* Kubernetes deployments

---

## 20. Next Project

Next:

```text
02-multi-stage-multi-agent/
```

Where we extend:

One container into Multiple containers per stage.

* Maven container
* Node container
* Database container

### Run This Pipeline

```bash
git clone <repo>
cd 01-my-first-pipeline
```

Configure Jenkins.

Run:

```text
Build Now
```

Validate:

```bash
docker ps
```

You have successfully implemented your first Jenkins pipeline using Docker as agent.

---