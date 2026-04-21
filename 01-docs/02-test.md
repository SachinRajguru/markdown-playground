# CI/CD – Study + Practical + Lab + Technical Guide

## Table of Contents
1. [Introduction to CI/CD](#1-introduction-to-cicd)
2. [What is CI/CD?](#2-what-is-cicd)
3. [Standard CI/CD Pipeline Steps (Lab Guide)](#3-standard-cicd-pipeline-steps-lab-guide)
4. [Detailed Step-by-Step Explanation](#4-detailed-step-by-step-explanation)
5. [Developer Workflow](#5-developer-workflow)
6. [Legacy CI/CD (Jenkins)](#6-legacy-cicd-jenkins)
7. [Environment Promotion](#7-environment-promotion)
8. [Historical Context of Jenkins](#8-historical-context-of-jenkins)
9. [Modern CI/CD (Cloud Native)](#9-modern-cicd-cloud-native)
10. [CI/CD Tool Ecosystem](#10-cicd-tool-ecosystem)
11. [Lab Preparation](#11-lab-preparation-next-step)
12. [Final Summary](#12-final-summary)

---

## 1. Introduction to CI/CD

CI/CD is one of the core topics in DevOps learning. It connects development → testing → deployment in an automated way.

### Session Goals
```bash
- Understand CI/CD fundamentals
- Learn legacy vs modern CI/CD setups
- Explore tools (Jenkins, GitHub Actions, etc.)
- Understand real-world workflows
- Prepare for hands-on implementation
```

**Note**: 
CI/CD bridges the gap between code written by developers and software delivered to customers, eliminating manual handoffs.

---

## 2. What is CI/CD?

CI/CD stands for:
```bash
CI → Continuous Integration
CD → Continuous Delivery / Continuous Deployment
```

### Continuous Integration (CI)

**Definition**: 
CI is the process of automatically integrating code changes into a shared repository with validation steps like testing and code analysis.

**Includes**:
```bash
- Testing
- Code analysis
- Quality checks
```

**Ensures**:
Code is correct, secure, and ready

**Simple Meaning**: 
Every time a developer pushes code → system automatically checks if it is correct.

**Practical Example**:
```bash
Developer pushes feature branch → CI runs tests → Merge to main if successful
```

### Continuous Delivery (CD)

**Definition**: 
CD is the process of automatically preparing and deploying application to different environments.

**Ensures:**
- App is delivered reliably
- Customer can access it

**Simple Meaning**: 
After code is tested → it is deployed to servers where users can access it.

### CI vs CD (Core Difference)

| Phase | Description            | Purpose             |
|-------|------------------------|---------------------|
| CI    | Build + Test + Scan    | Ensure code quality |
| CD    | Deploy to environments | Deliver to users    |

**Simple Formula**:
```bash
CI = Build + Test + Scan
CD = Deploy (Dev → Staging → Production)
```

**Restaurant Analogy**
```bash
CI = Chef prepares and checks food quality (taste, hygiene, presentation)
CD = Food is served to customer tables (reliable delivery)
```

**Textbook Summary**
```bash
CI automates testing and validation
CD automates deployment and delivery
```

### 🌍 Real-World Scenario
**Situation**:
- Developer builds app on laptop 💻
- Customer is in another country 🌍

**❓ Problem**: 
`How does code reach customer reliably?`

### ➤ Required Steps (Standard Flow)
Every organization follows these core steps:
1. Unit Testing 
2. Static Code Analysis 
3. Code Quality & Security Scan 
4. Automation / E2E Testing 
5. Reporting 
6. Deployment 

**⚠️ Important Insight**
- Steps vary by application type:
    - Web apps
    - Mobile apps
    - Government/secure apps
➤ **But core pipeline always exists**

### ✅ Solution
```bash
Automate everything using CI/CD
CI/CD = Automation of entire delivery pipeline
```
### ⚠️ Why Automation?

| Without CI/CD      | With CI/CD       |
|--------------------|------------------|
| Months to release  | Hours/days       |
| Manual testing     | Automated        |
| Human errors       | Consistent execution |

---

## 3. Standard CI/CD Pipeline Steps (Lab Guide)

**Pipeline Flow**
```bash
Developer → VCS → CI/CD Tool → Customer
```

**Steps Involved**
```bash
1. Unit Testing
2. Static Code Analysis
3. Code Quality / Security Testing
4. Automation Testing (E2E)
5. Reporting
6. Deployment
```

---

## 4. Detailed Step-by-Step Explanation

### 4.1 Unit Testing

**Definition**: 
Testing individual function or block of code

**Example**:
```python
# calculator.py
def add(a, b):
    return a + b

# test_calculator.py
def test_add():
    assert add(2, 3) == 5  # Expected output: PASS ✅
    assert add(-1, 1) == 0 # Edge case: PASS ✅
```

**Concept**:
```python
Testing only: add(2,3) → 5 ✅
```

**⚠️ Why automate?**
- Developers make hundreds of changes
- Manual testing = ❌ Impossible
- Must be automated

**Interview Q&A**
```bash
Q: What is unit testing?
A: Testing a single function independently (e.g., add(2,3)=5)
```

**Lab Exercise**:
Write 3 unit tests for a `multiply(a,b)` function.

### 4.2 Static Code Analysis

**Definition**:
Checking code without running it.

**What it checks**:
- Syntax errors
- Formatting
- Indentation
- Unused variables

**Example**:
```python
# ❌ Bad practice - Static Analysis will flag this
a = 1
b = 2
c = 3  # Unused variable ⚠️
d = 4  # Unused variable ⚠️
...
z = 26 # Unused variable ⚠️

result = a + b  # only 2 variables used
```

**❌ Problem**:
- Memory wasted
- Poor coding practice

**Tools**

| Tool      | Language   | Purpose                    |
| --------- | ---------- | -------------------------- |
| SonarQube | All        | Code smells, bugs          |
| ESLint    | JavaScript | Formatting, best practices |
| Pylint    | Python     | Style, unused vars         |

**Analogy**:
Like Grammar check before publishing article.

**Interview Q&A**
```bash
Q: Why static analysis?
A: Detects issues before runtime (memory, syntax, formatting)
```

**Lab Exercise**:
Install `Pylint` and analyze the bad code example above.

### 4.3 Code Quality & Security Testing

**Definition**: 
Ensures code is:
- Secure
- Reliable
- Free from vulnerabilities

**Real Example**:
```bash
New Android update released → Vulnerability discovered → Hackers exploit it
```

**Example Report**:
```bash
Vulnerability: Log4Shell (CVE-2021-44228)
Severity: CRITICAL ⚠️
Fix: Upgrade log4j to 2.17.1
```
*Vulnerability detected → Deployment blocked ➤ Bad user experience*

**Goal**:
Prevent insecure deployments

**Tools**

| Tool       | Purpose                                    |
|------------|--------------------------------------------|
| Snyk       | Dependency vulnerabilities                 |
| OWASP      | Security scanning                          |
| Checkmarx  | SAST (Static Application Security Testing) |
| Dependabot | Auto PRs for fixes                         |

**Interview Q&A**
```bash
Q: Why is vulnerability testing important?
A: Prevents insecure applications from reaching users
```

**Lab Exercise**:
Scan a sample Java project with Snyk for Log4j vulnerabilities.

### 4.4 Automation / End-to-End Testing

**Definition**:
Testing entire application flow

**Example Comparison**

| Test Type | Example Flow                            |
|-----------|-----------------------------------------|
| Unit Test | `add(2,3)=5` ✅                        |
| E2E Test  | `Login → Calculator → Add → Logout` ✅ |

**Concept**:
```bash
If you change addition function, verify:
• Subtraction works
• Multiplication works
• Division works
```
*Ensure new changes don't break existing features*

**Tools**

| Tool     | Use Case    |
|----------|-------------|
| Selenium | Web         |
| Appium   | Mobile      |
| Cypress  | Modern Web  |

**Difference Table**

| Type       | Scope              |
|------------|--------------------|
| Unit Test  | Single function    |
| E2E Test   | Entire application |

**Analogy**:
```bash
Unit → Testing engine separately
E2E → Driving full car on road
```

**Interview Q&A**
```bash
Q: Unit vs Functional Testing?
A: Unit = single function, Functional = end-to-end flow
```

**Lab Exercise**:
Write a Cypress test for a login → calculator → logout flow.

### 4.5 Reporting

**Definition**:
Storing results of pipeline execution

**Example Metrics**
```bash
Unit Coverage: 90% ✅
E2E Passed: 48/50 ✅
Security: No critical issues ✅
```

**❓ Why Important?**
```bash
- Audit + debugging + compliance
```

**❓ Why needed?**
```bash
Management may ask:
• "How many tests passed?"
• "Is code secure?"
```

**Tools**:

| Tool          | Use Case               |
|---------------|------------------------|
| Allure        | Beautiful test reports |
| ExtentReports | Detailed HTML reports  |
| ELK Stack     | Log aggregation        |

**Lab Exercise**:
Generate an Allure report from pytest results.

### 4.6 Deployment

**Definition**: 
Making application accessible to users
- Deploys application to environments
- Makes app accessible to users

**⚠️ Truth**: 
`Code on your laptop ≠ usable by customer`

**⚠️ Mandatory Step**: 
Without deployment → no user access

**Deployment Targets**
```bash
- Cloud VMs
- Containers
- Kubernetes
- Servers
```

**Interview Q&A**
```bash
Q: Why not deploy directly to production?
A:
    - Risk reduction
    - Cost optimization
    - Proper testing before release
```

**Lab Exercise**:
Deploy a sample app to a Kubernetes dev namespace.

---

## 5. Developer Workflow

### 🔁 Modern Workflow
```bash
Small Changes → Frequent Commits → Automated Pipeline
```

### ❌ Myth vs Reality

| Myth                        | Reality                         |
|-----------------------------|---------------------------------|
| Deploy once after full feature | Continuous small deployments |

### Workflow Example: (Jira + Git Workflow)
```bash
Sprint – Jira Stories:

Story #1: Version v1.0 → Commit → Pipeline runs ✓
Story #2: Version v2.0 → Commit → Pipeline runs ✓
...
Final → Production Ready ✓
```

### 🔔 Trigger Mechanism
```bash
Git Push → Automatically triggers CI/CD
```

### 📦 Storage: Version Control System (VCS)

**Definition**:
Store and manage code versions.

**Tools**:
```bash
- GitHub
- GitLab
- Bitbucket
```

### CI/CD Pipeline

**Definition**:
Automated sequence of steps executed after code push.

### 🔄 Workflow Overview
```bash
1. Developer writes code
2. Pushes to VCS (GitHub/GitLab/Bitbucket)
3. CI/CD pipeline triggers automatically
4. All steps (test → scan → deploy) run automatically
```

---

## 6. Legacy CI/CD (Jenkins)

### 🧰 Tool: Jenkins

### 🏗️ Architecture
```bash
Developer → GitHub → Jenkins → Tools → Deployment
```

**Explanation**:
```bash
1. Developer writes code
2. Pushes to GitHub
3. Jenkins detects changes
4. Triggers pipeline
5. Executes tools (build, test, deploy)
```

### 🔄 CI/CD Flow (Step-by-Step)

**Trigger Mechanism**
```bash
Jenkins continuously:
- Watches GitHub repository
- Triggers pipeline on:
  - Commit
  - Pull Request
```

**Execution Flow**
```bash
1. Developer pushes code → GitHub
2. Jenkins detects change (via webhook/polling)
3. Jenkins triggers pipeline
4. Pipeline executes stages (Build → Test → Deploy)
```

### 📌 What is Jenkins?
Jenkins is an automation server used to build CI/CD pipelines.

**Core Role**: `Orchestrator`

**Important Concept**:
```bash
❌ Jenkins does NOT do everything itself
✅ Jenkins CONNECTS and RUNS tools
```

**Responsibilities**:
```bash
- Orchestrates pipeline stages
- Connects different tools
- Executes workflow
- Automates entire CI/CD process
```

**Tools Integrated with Jenkins**

| Stage         | Tool                  |
|---------------|-----------------------|
| Build         | Apache Maven          |
| Unit Testing  | JUnit                 |
| Code Coverage | JaCoCo                |
| Code Quality  | SonarQube             |
| Reporting     | ALM Tools             |
| Deployment    | Docker/Kubernetes/EC2 |

**Key Concept** (VERY IMPORTANT)
```bash
❗ Misconception: "Jenkins runs tests"
✅ Reality: Jenkins triggers tools (like JUnit) to execute tests
```

**Real-World Analogy**
```bash
Jenkins = Project Manager 👨‍💼
├── Developer = Worker
├── Tools = Specialized employees
└── Jenkins tells others what to do and when
```

### Jenkins Pipeline Example

```groovy
pipeline {
    agent any   // Run on any available Jenkins agent/node
    
    stages {
        stage('Build') {  // Compile and package stage
            steps {
                // Compile Java code and create JAR/WAR
                sh 'mvn clean install'
            }
        }
        
        stage('Test') {   // Run automated tests
            steps {
                // Execute JUnit tests + generate reports
                sh 'mvn test'
            }
        }
        
        stage('Deploy') { // Deploy to target environment
            steps {
                // Apply Kubernetes manifests
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
```

### Pipeline Breakdown

| Component         | Explanation                              |
|-------------------|------------------------------------------|
| `agent any`       | Run pipeline on any available node/agent |
| `stage('Build')`  | Uses Maven to compile + package          |
| `stage('Test')`   | Runs JUnit tests                         |
| `stage('Deploy')` | Deploys using Kubernetes                 |

### Practical Scenario
**Imagine working at a company**:
```bash
1. You push code
2. Jenkins automatically:
   ├── Builds your app
   ├── Runs tests
   ├── Checks code quality
   └── Deploys to server

➤ No manual work → Fully automated!
```

### Interview Q&A

**Q1**: What is Jenkins?
```bash
A: Jenkins is an open-source automation server used to build, test, and deploy 
   applications by orchestrating CI/CD pipelines.
```
**Q2**: Does Jenkins execute tests?
```bash
A: No. Jenkins orchestrates tools like JUnit that execute tests.
```

**Q3**: What triggers a Jenkins pipeline?
```bash
A: Code commits, Pull requests, Webhooks, Scheduled jobs
```

**Q4**: What is a Jenkins Pipeline?
```bash
A: A Jenkins Pipeline is a set of automated steps defined as code (Jenkinsfile) 
   that describes the CI/CD process.
```

**Q5**: What is the role of Jenkins in CI/CD?
```bash
A: Jenkins acts as an orchestrator that connects and automates different tools 
   in the CI/CD lifecycle.
```

### Final Summary (Jenkins)
```bash
• Jenkins = Automation Server
• Main Role = Orchestrator
• Works with tools (not replaces them)
• Automates entire CI/CD pipeline
• Triggered by GitHub changes
```

---

## 7. Environment Promotion

**CI/CD Flow with Environments**
```bash
Developer → GitHub → Jenkins → Environments (DEV → STAGING → PRODUCTION)
```

**Key Principles**:
```bash
- Code moves progressively forward
- Each environment adds more validation + stability
- Code flows through multiple environments
- Each environment increases in: Complexity, Stability, User exposure
```

### Environments Overview

#### 1️. Dev Environment

**Purpose**:
Used by developers for initial development and quick testing

**Characteristics**:
- Simple setup
- Fast deployments
- Less strict validation

**Example**:
```bash
• 1 server/container
• Local DB or mock services
• Frequent deployments (multiple times/day)
```

#### 2️. Staging Environment

- **Purpose**:
Simulates production for final testing before release

- **Characteristics**:
    - Medium complexity
    - Mirrors production closely
    - Used by QA team/testers

-**Used For**:
    - Integration testing
    - QA testing
    - Bug validation
    - UAT (User Acceptance Testing)
    - Pre-release checks

**Example**:
```bash
• Few servers
• Realistic configurations
• Connected services
```

#### 3️. Production Environment

- **Purpose**:
Live application serving real customers

- **Characteristics**:
    - Full-scale system
    - High availability
    - Used by real users
    - Strict controls (approvals, monitoring, rollback)

- **Features**:
    - Auto-scaling enabled
    - Load balancing
    - Continuous monitoring & alerting

**Example**:
```bash
• Large cluster
• Load balancers
• Auto-scaling infrastructure
• Monitoring (Prometheus, Grafana)
```

#### Infrastructure Comparison

| Environment | Infrastructure Setup                       |
|-------------|--------------------------------------------|
| Dev         | 1 server/container                         |
| Staging     | Few servers (production-like)              |
| Production  | Large cluster (auto-scaled, load-balanced) |

- **❓ Why NOT Use Production Everywhere?**

| Reason    | Explanation                                   |
|-----------|-----------------------------------------------|
| 💸 Cost  | Production infra expensive, wasteful for tests |
| ⚠️ Risk  | Bugs break live system, affect real users      |
| 🐢 Speed | Slower for dev/testing, strict controls        |

- **Key Concept**: `Environment Promotion`
```bash
Build once → Deploy everywhere
```

- **Code promoted step-by-step**: Dev → Staging → Production

- Core Principles:
    - Code built ONCE
    - Same code deployed across environments
    - Confidence ↑ at each stage
    - Risk ↓ before production

**Benefits**
- Early bug detection
- Safer releases
- Higher confidence in deployments

**Real-World Analogy**
```bash
🎓 Education System:
    Dev → Practice at home
    Staging → Mock exam
    Production → Final exam

➤ You don't go directly to final exam!
```

---

## 8. Historical Context of Jenkins

### 📅 Evolution Timeline
```
• 2004 → Hudson created
• 2011 → Jenkins introduced (fork from Hudson)
```

### 🏗️ Infrastructure Evolution
```bash
🖥️ On-Premises Servers → ☁️ Cloud VMs → ⚠️ Today (Microservices/Cloud-Native)
```

**⚠️ Traditional Jenkins struggles with**:
```bash
- Dynamic workloads
- Microservices architecture
- Container orchestration
```

## ⚠️ Problems with Legacy CI/CD (Jenkins)

### 1️. High Infrastructure Cost 💸

**Problem**:
```bash
Requires:
├── Master node (always running)
└── Multiple worker nodes (always running)
```

**Resource Usage** (24/7):
- CPU
- RAM
- Storage

**❗ Even when idle → consuming resources**

**Result**:
- High cloud bills
- Wasted infrastructure

### 2️. Scaling is Manual ⚙️

**Problem**:
```bash
Manual tasks:
├── Add/remove worker nodes
└── Configure infrastructure
```

**❗ Challenges**:
- Time-consuming
- Requires DevOps effort
- Not instant

**Result**: 
Slow, inefficient scaling

### 3️. No Scale-to-Zero 🚫

**Problem**:
```bash
Jenkins master MUST always run
Even when:
├── No builds
├── No deployments
└── Weekends/nights
```

**Result**: Paying for idle infrastructure

### 4️. Maintenance Overhead
```bash
• Plugin management
• Version compatibility issues
➤ Operational complexity + frequent maintenance
```

### Real-World Scenario
```bash
Organization: 30–40 VMs running Jenkins
During nights 🌙 / weekends 🛌 / no deployments
➤ Still paying FULL cost!
```

### Analogy
```bash
Legacy Jenkins = Office always open 💡
├── Lights always ON
├── Staff always present 👨‍💼
└── Even when no work happening
```

### Core Limitation: 
```bash
❌ Cannot scale down to zero
```

### Final Summary (Legacy Problems)
```bash
CI/CD uses environment promotion for safe delivery
Jenkins designed for:
├── Static infrastructure
└── Long-running servers

❗ Major Limitations:
• 💸 High cost
• ⚙️ Manual scaling
• 🚫 No scale-to-zero
```

---

## 9. Modern CI/CD (Cloud Native)

**🧰 Example Stack**
```bash
- Kubernetes
- GitHub Actions
```

### GitHub Actions

**Definition**: 
GitHub Actions is an **event-driven CI/CD platform integrated with GitHub**.

**Key Features**:
- Native GitHub integration
- Auto-triggers on:
  ```
  ├── Commits
  └── Pull Requests
  ```

### Key Concept: Ephemeral Compute

**What It Means**:
```bash
Traditional: Servers run 24/7 🛑
Modern: Compute created only when needed ✅
```

**Execution Flow**:
```bash
Start Container → Run Pipeline → Destroy Container
```

**Step-by-Step Workflow**:
```bash
1. Developer pushes code
2. Event triggers workflow
3. Container/Pod spins up
4. Pipeline runs (build/test/deploy)
5. Container destroyed ✅
```

**Result**:
```bash
✅ Zero idle infrastructure
✅ Cost-efficient
✅ Faster builds
```

### Core Concept
```bash
Uses: Shared infrastructure + Ephemeral compute
➤ No always-on servers
➤ Pay only when pipeline runs
```

### Analogy
```bash
Like Uber 🚗:
├── No need to own car
├── Use only when needed
└── Pay only for usage

➤ Same as modern CI/CD compute!
```

### Advantages of Modern CI/CD

| Feature             | Benefit                |
|---------------------|------------------------|
| Zero idle cost      | Pay-per-use            |
| Highly scalable     | Auto-scaling           |
| Shared infra        | Efficient utilization  |
| Faster execution    | Ephemeral containers   |
| Native integrations | GitHub/GitLab built-in |

## Real-World Example (Kubernetes-Scale Projects)

**Scenario**:
```bash
- Thousands of contributors
- Global collaboration
- Frequent code changes
```

**Workflow**:
```bash
1. Developer creates PR
2. GitHub Actions triggers
3. Pipeline runs in container
4. Container destroyed
```

**Key Benefit**:
✅ Zero compute when idle

### 🔄 Shared Resource Concept
```bash
• One cluster for multiple projects
• Dynamic resource allocation
• Efficient utilization
```

### ⚖️ Jenkins vs Modern CI/CD

**🆚 Detailed Comparison**

| Feature          | Jenkins 🏗️         | GitHub Actions ⚡  |
|------------------|--------------------|---------------------|
| Setup            | Manual             | Built-in            |
| Scaling          | Manual             | Automatic           |
| Cost             | High (always-on)   | Pay-per-use         |
| Idle Resources   | Always running     | Zero when idle      |
| Architecture     | VM-based           | Container-based     |
| Infrastructure   | Dedicated          | Shared/Ephemeral    |
| Maintenance      | High               | Low (managed)       |
| Execution        | Long-running       | On-demand           |
| Event-Driven     | Limited            | Native              |

**Insight**:
```bash
Jenkins → Powerful but heavy 🏗️
GitHub Actions → Lightweight & scalable ⚡
```

### Key Modern CI/CD Features
```bash
• Event-driven pipelines
• Ephemeral infrastructure
• Auto-scaling
• Cross-project integration
• Cloud-native design
```

---

## 10. CI/CD Tool Ecosystem

**Legacy**
```bash
• Jenkins (self-hosted, powerful, complex)
```

**Modern**
```bash
• GitHub Actions (GitHub-native)
• GitLab CI/CD (GitLab-native)
• CircleCI (cloud-first)
• Travis CI (open source focus)
```

**Important Insight**:
```bash
Most CI/CD tools conceptually similar
➤ Main difference = syntax + ecosystem
```

**Note**:
Choose based on your VCS (GitHub → Actions, GitLab → CI/CD)

---

## 11. Lab Preparation (Next Step)

**Upcoming Hands-On Labs**
```bash
1. Jenkins setup (Docker/local)
2. Pipeline creation (Jenkinsfile)
3. GitHub Actions demo (YAML workflows)
4. End-to-end CI/CD project
   ├── Simple web app
   ├── Full pipeline (build→test→deploy)
   └── Multi-environment promotion
```

**Prerequisites**:
```bash
• Docker installed
• GitHub account
• Basic YAML/Groovy knowledge
• Sample application (Node.js/Python)
```

---

## 12. Final Summary

**🔑 Key Takeaways**
```bash
CI/CD = Automation of software delivery pipeline

Pipeline includes:
├── Build (Maven/Webpack)
├── Test (Unit + E2E)
└── Deploy (Containers/K8s)

Jenkins = Legacy orchestrator (still important)
├── Pros: Powerful, flexible
└── Cons: Heavy, costly

Modern CI/CD = Cloud-native, scalable, cost-efficient
├── GitHub Actions, GitLab CI
├── Ephemeral compute
└── Pay-per-use model
```

**Final Insight: Industry Shift**
```bash
➤ Moving from:
Static Infrastructure (Jenkins) 🏗️
        ↓
Dynamic, On-Demand Systems (Cloud CI/CD) ⚡
    └── GitHub Actions + Kubernetes
```

**Remember**: 
```bash
Legacy = Office always open 💰
Modern = Uber - pay per ride 🚗
```

---

## 🎓 Interview Preparation Questions

**Q1**: Explain CI/CD pipeline stages?
```bash
A: Build → Unit Test → Static Analysis → Security Scan → E2E Test → Report → Deploy
```

**Q2**: Jenkins vs GitHub Actions?
```bash
A: Jenkins=heavy self-hosted orchestrator, GitHub Actions=lightweight cloud-native event-driven
```

**Q3**: What is environment promotion?
```bash
A: Dev→Staging→Production progression with increasing validation/stability
```

**Q4**: Why ephemeral compute?
```bash
A: Zero idle cost, auto-scaling, pay-per-use efficiency
```

**Ready for Hands-On Labs!** 