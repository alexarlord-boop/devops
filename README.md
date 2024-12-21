# My DevOps path

This repo is a showcase of my knowledge in devops practices and tools. 
It is a collection of notes, scripts, and configurations that I have used or created while learning and working in the field of DevOps.

All steps were implemented with a security-first mindset, following best practices and standards with DevSecOps tools for automated security checks.

### Table of contents
1. [DevOps Pre-Requisites](#devops-pre-requisites)
2. [DevOps Fundamentals](#devops-fundamentals)
3. [DevOps Core](#devops-core)
4. [DevOps Advanced](#devops-advanced)



## DevOps Pre-Requisites
🅰. **OS & Linux fundamentals**, like:
  1. Shell commands
  2. Shell scripting
  3. File permissions
  4. SSH key management
  5. Networking
  6. Virtualization

Operating systems are basically foundation of computing environments. Whether you 
want to run pipelines or spin up servers for Kubernetes it's all powered by Linux.
Most Docker containers use Linux images and as a DevOps engineer you will work most
of the time with remote servers on cloud which use Linux operating system.

🅱. **Git essentials** that let teams collaborate on code efficiently, like:
  1. Common commands
  2. Branching strategies (gitflow, trunk-based)
  3. Merging
  4. Resolving conflicts
  5. Git role in "X as Code"

Git is needed across multiple tools and DevOps concepts, whether you are
  a. writing a pipeline or infrastructure configuration
  b. writing Kubernetes manifest files or application code

🅲. **Build tools & Package managers**, essential for:
  1. Installing dependecies
  2. Executing tests
  3. Packaging application

Build tools like Maven, Gradle, or npm streamline the process of compiling, testing, and packaging applications, ensuring consistency across environments. Package managers such as apt, yum, pip, and npm simplify dependency management, making it easier to install, update, and maintain the libraries or tools your applications require.


## DevOps Fundamentals
🅰. **Containerization with Docker**

🅱. **Artifact repository**

🅲. **Cloud basics**
  1. Compute with VMs
  2. Storage
  3. Networking & Security


## DevOps Core
🅰. **Container orchestration - Kubernetes**

🅱. **Advanced cloud platform skills (AWS)**
  1. Design, build, maintain complex cloud infrastructure
  2. Scale cloud infrastructure
  3. Deploy and manage clusters
  4. Managed Kubernetes cluster: AWS EKS

🅲. **CI/CD**
  1. Setting up CI/CD server
  2. Integrate code repo to trigger pipeline
  3. Build, test, deploy automation

## DevOps Advanced
🅰. **Infrastructure as Code** (IaC) with Terraform

🅱. **Automation with Python** (monitoring, scheduling, backups, etc.)

🅲. **Configuration management** with Ansible

🅳. **Monitoring & Observability**
  1. Monitoring tools like Prometheus, Grafana
  2. Logging tools like ELK stack