# DevOps Interview Preparation

A comprehensive guide to common DevOps interview questions and answers.

## 📋 Table of Contents

- [General DevOps Concepts](#general-devops-concepts)
- [Git & Version Control](#git--version-control)
- [Linux & Scripting](#linux--scripting)
- [Docker & Containers](#docker--containers)
- [Kubernetes](#kubernetes)
- [CI/CD](#cicd)
- [Infrastructure as Code](#infrastructure-as-code)
- [Cloud Platforms](#cloud-platforms)
- [Monitoring & Logging](#monitoring--logging)
- [Security](#security)
- [Behavioral Questions](#behavioral-questions)
- [System Design](#system-design)

---

## General DevOps Concepts

### Q1: What is DevOps?
**Answer**: DevOps is a set of practices that combines software development (Dev) and IT operations (Ops) to shorten the development lifecycle and provide continuous delivery with high quality. It emphasizes:
- Automation
- Collaboration
- Continuous Integration/Continuous Deployment
- Monitoring and feedback
- Rapid iteration

### Q2: What are the key principles of DevOps?
**Answer**:
1. **Automation**: Automate repetitive tasks
2. **Continuous Integration**: Integrate code frequently
3. **Continuous Delivery**: Deploy frequently and reliably
4. **Monitoring**: Continuous monitoring of applications and infrastructure
5. **Collaboration**: Breaking down silos between teams
6. **Infrastructure as Code**: Manage infrastructure through code
7. **Feedback Loops**: Quick feedback from production to development

### Q3: What is the difference between Continuous Integration, Continuous Delivery, and Continuous Deployment?
**Answer**:
- **CI (Continuous Integration)**: Automatically building and testing code every time a change is committed
- **CD (Continuous Delivery)**: Code is always in a deployable state; deployment to production requires manual approval
- **CD (Continuous Deployment)**: Every change that passes tests is automatically deployed to production

### Q4: What are microservices?
**Answer**: Microservices is an architectural style where an application is built as a collection of small, independent services that:
- Run in their own processes
- Communicate via APIs (usually HTTP/REST or message queues)
- Can be deployed independently
- Are organized around business capabilities
- Can use different technologies/languages

**Benefits**:
- Scalability
- Flexibility
- Easier maintenance
- Faster deployment

**Challenges**:
- Increased complexity
- Distributed system challenges
- Monitoring difficulties
- Data consistency

---

## Git & Version Control

### Q5: What is Git rebase and how is it different from merge?
**Answer**:
- **Merge**: Combines branches by creating a new merge commit, preserving history
- **Rebase**: Moves or combines commits to a new base, creating a linear history

**When to use**:
- **Merge**: For integrating feature branches into main
- **Rebase**: For cleaning up local commits before pushing

```bash
# Merge
git merge feature-branch

# Rebase
git rebase main
```

### Q6: What is a Git conflict and how do you resolve it?
**Answer**: A conflict occurs when Git cannot automatically merge changes because the same lines were modified in different branches.

**Resolution steps**:
1. Identify conflicted files: `git status`
2. Open files and look for conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
3. Manually edit to keep desired changes
4. Remove conflict markers
5. Stage resolved files: `git add <file>`
6. Complete merge/rebase: `git commit` or `git rebase --continue`

### Q7: What are Git hooks?
**Answer**: Git hooks are scripts that run automatically at specific points in the Git workflow.

**Common hooks**:
- `pre-commit`: Runs before commit (linting, tests)
- `pre-push`: Runs before push
- `post-commit`: Runs after commit
- `pre-receive`: Runs on server before accepting push

**Example use cases**:
- Code formatting
- Running tests
- Preventing commits to protected branches
- Sending notifications

---

## Linux & Scripting

### Q8: How do you check disk space usage on Linux?
**Answer**:
```bash
# Overall disk usage
df -h

# Directory sizes
du -sh /path/*

# Largest directories
du -h /path | sort -rh | head -10
```

### Q9: How do you find and kill a process?
**Answer**:
```bash
# Find process
ps aux | grep <process-name>
# or
pgrep <process-name>

# Kill process
kill <PID>
kill -9 <PID>  # Force kill

# Kill by name
killall <process-name>
pkill <process-name>
```

### Q10: What is the difference between hard link and soft link?
**Answer**:
- **Hard Link**: Direct reference to the inode; deleting original doesn't affect hard link
- **Soft Link (Symbolic)**: Pointer to filename; breaks if original is deleted

```bash
# Hard link
ln original.txt hardlink.txt

# Soft link
ln -s original.txt softlink.txt
```

### Q11: Explain file permissions in Linux
**Answer**: Permissions are represented by 10 characters: `-rwxrwxrwx`

- First character: File type (`-` file, `d` directory)
- Next 3: Owner permissions (read, write, execute)
- Next 3: Group permissions
- Last 3: Others permissions

**Numeric representation**:
- Read (r) = 4
- Write (w) = 2
- Execute (x) = 1

```bash
chmod 755 file  # rwxr-xr-x
chmod 644 file  # rw-r--r--
```

### Q12: How do you troubleshoot high CPU usage?
**Answer**:
1. Identify process: `top` or `htop`
2. Check specific process: `ps aux | grep <PID>`
3. View threads: `top -H -p <PID>`
4. Check system logs: `dmesg`, `journalctl`
5. Use profiling tools: `perf`, `strace`

---

## Docker & Containers

### Q13: What is the difference between a Docker image and a container?
**Answer**:
- **Image**: Read-only template containing application code, libraries, and dependencies. Built from Dockerfile.
- **Container**: Running instance of an image. Writable layer on top of image.

```bash
# Image is like a class, container is like an object
docker run <image>  # Creates container from image
```

### Q14: Explain Docker networking modes
**Answer**:
1. **Bridge** (default): Private network on host, containers communicate via bridge
2. **Host**: Container shares host's network namespace, no isolation
3. **None**: No networking, complete isolation
4. **Overlay**: Multi-host networking for Swarm
5. **Macvlan**: Assign MAC address to container, appears as physical device

```bash
docker run --network=bridge myapp
docker run --network=host myapp
```

### Q15: How do you reduce Docker image size?
**Answer**:
1. **Use smaller base images**: Alpine instead of Ubuntu
2. **Multi-stage builds**: Separate build and runtime stages
3. **Minimize layers**: Combine RUN commands
4. **Remove unnecessary files**: Clean cache, temp files
5. **Use .dockerignore**: Exclude unnecessary files

```dockerfile
# Multi-stage build example
FROM node:16 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:16-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY package*.json ./
RUN npm ci --production
CMD ["node", "dist/index.js"]
```

### Q16: What is Docker Compose and when do you use it?
**Answer**: Docker Compose is a tool for defining and running multi-container applications using YAML files.

**Use cases**:
- Development environments
- Automated testing
- Single-host deployments
- Microservices architecture locally

**Example**:
```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "8080:80"
  db:
    image: postgres:13
    environment:
      POSTGRES_PASSWORD: secret
```

---

## Kubernetes

### Q17: What is Kubernetes and why use it?
**Answer**: Kubernetes is an open-source container orchestration platform that automates deployment, scaling, and management of containerized applications.

**Benefits**:
- Auto-scaling
- Self-healing
- Load balancing
- Rolling updates
- Service discovery
- Storage orchestration

### Q18: Explain the architecture of Kubernetes
**Answer**:

**Control Plane (Master)**:
- **API Server**: Frontend for K8s control plane
- **etcd**: Key-value store for cluster data
- **Scheduler**: Assigns pods to nodes
- **Controller Manager**: Runs controller processes
- **Cloud Controller Manager**: Interacts with cloud provider

**Worker Nodes**:
- **kubelet**: Ensures containers are running
- **kube-proxy**: Network proxy
- **Container Runtime**: Docker, containerd, CRI-O

### Q19: What is the difference between a Pod, Deployment, and StatefulSet?
**Answer**:

**Pod**: Smallest deployable unit, contains one or more containers
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14
```

**Deployment**: Manages stateless applications, provides rolling updates, rollbacks
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  template:
    spec:
      containers:
      - name: nginx
        image: nginx:1.14
```

**StatefulSet**: For stateful applications, guarantees ordered deployment and stable network IDs
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: mysql
  replicas: 3
  template:
    spec:
      containers:
      - name: mysql
        image: mysql:5.7
```

### Q20: What are Kubernetes Services and their types?
**Answer**: Service is an abstraction that exposes applications running as Pods.

**Types**:
1. **ClusterIP** (default): Internal cluster IP, only accessible within cluster
2. **NodePort**: Exposes service on each node's IP at a static port
3. **LoadBalancer**: Exposes service externally using cloud provider's load balancer
4. **ExternalName**: Maps service to DNS name

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: LoadBalancer
  selector:
    app: MyApp
  ports:
  - port: 80
    targetPort: 8080
```

### Q21: How do you troubleshoot a pod that won't start?
**Answer**:
```bash
# Check pod status
kubectl get pods
kubectl describe pod <pod-name>

# Check logs
kubectl logs <pod-name>
kubectl logs <pod-name> --previous  # Previous container

# Check events
kubectl get events --sort-by='.lastTimestamp'

# Execute into pod (if running)
kubectl exec -it <pod-name> -- /bin/sh

# Common issues:
# - Image pull errors (wrong image, auth)
# - Resource limits
# - ConfigMap/Secret not found
# - Volume mount issues
```

---

## CI/CD

### Q22: What is a CI/CD pipeline?
**Answer**: A CI/CD pipeline is an automated workflow that builds, tests, and deploys code changes.

**Typical stages**:
1. **Source**: Checkout code from repository
2. **Build**: Compile and package application
3. **Test**: Run unit, integration, security tests
4. **Deploy to Staging**: Deploy to test environment
5. **Deploy to Production**: Deploy to production (manual or automatic)
6. **Monitor**: Track deployment health

### Q23: What is Blue-Green Deployment?
**Answer**: Deployment strategy where two identical production environments (Blue and Green) exist.

**Process**:
1. Blue is currently live
2. Deploy new version to Green
3. Test Green thoroughly
4. Switch traffic from Blue to Green
5. Blue becomes backup/rollback option

**Benefits**:
- Zero downtime
- Easy rollback
- Full testing before switch

**Drawbacks**:
- Requires double resources
- Database migrations can be complex

### Q24: What is Canary Deployment?
**Answer**: Gradually rolling out changes to a small subset of users before full deployment.

**Process**:
1. Deploy new version to small percentage (e.g., 5%)
2. Monitor metrics and errors
3. Gradually increase traffic (10%, 25%, 50%, 100%)
4. Rollback if issues detected

**Benefits**:
- Reduced risk
- Real user testing
- Easy rollback

### Q25: How do you handle secrets in CI/CD?
**Answer**:
1. **Never commit secrets to Git**
2. **Use secret management tools**:
   - HashiCorp Vault
   - AWS Secrets Manager
   - Azure Key Vault
   - Kubernetes Secrets
3. **Environment variables** in CI/CD tool
4. **Rotate secrets regularly**
5. **Use least privilege principle**

**Example (GitHub Actions)**:
```yaml
steps:
  - name: Deploy
    env:
      API_KEY: ${{ secrets.API_KEY }}
    run: |
      echo "Deploying with secret"
```

---

## Infrastructure as Code

### Q26: What is Infrastructure as Code (IaC)?
**Answer**: Managing and provisioning infrastructure through code instead of manual processes.

**Benefits**:
- **Version control**: Track infrastructure changes
- **Consistency**: Same configuration every time
- **Automation**: Rapid provisioning
- **Documentation**: Code serves as documentation
- **Disaster recovery**: Rebuild infrastructure quickly

**Tools**: Terraform, Ansible, CloudFormation, Pulumi

### Q27: What is the difference between Terraform and Ansible?
**Answer**:

**Terraform**:
- Infrastructure provisioning
- Declarative (describe desired state)
- Cloud-agnostic
- State management
- Plan before apply

**Ansible**:
- Configuration management
- Procedural (sequence of steps)
- Agentless (SSH-based)
- No state file
- Idempotent playbooks

**Use together**: Terraform for infrastructure, Ansible for configuration

### Q28: Explain Terraform state file
**Answer**: State file (`terraform.tfstate`) tracks resources Terraform manages.

**Purpose**:
- Maps configuration to real resources
- Stores metadata
- Performance optimization

**Best practices**:
- **Use remote state**: S3, Azure Storage, Terraform Cloud
- **Enable state locking**: Prevent concurrent modifications
- **Never edit manually**
- **Backup regularly**
- **Use workspace** for multiple environments

```hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "prod/terraform.tfstate"
    region = "us-east-1"
    encrypt = true
    dynamodb_table = "terraform-locks"
  }
}
```

---

## Cloud Platforms

### Q29: What are the main components of AWS?
**Answer**:

**Compute**: EC2, Lambda, ECS, EKS
**Storage**: S3, EBS, EFS
**Database**: RDS, DynamoDB, Aurora
**Networking**: VPC, Route 53, CloudFront, ELB
**Security**: IAM, Security Groups, KMS
**Monitoring**: CloudWatch, CloudTrail
**DevOps**: CodePipeline, CodeBuild, CodeDeploy

### Q30: What is AWS VPC?
**Answer**: Virtual Private Cloud - isolated network within AWS cloud.

**Components**:
- **Subnets**: Public and private
- **Internet Gateway**: Access to internet
- **NAT Gateway**: Outbound internet for private subnets
- **Route Tables**: Traffic routing
- **Security Groups**: Instance-level firewall
- **NACLs**: Subnet-level firewall

### Q31: What is the difference between horizontal and vertical scaling?
**Answer**:

**Horizontal Scaling (Scale Out)**:
- Add more instances/servers
- Better for cloud environments
- Higher availability
- More complex (load balancing, state management)

**Vertical Scaling (Scale Up)**:
- Increase resources (CPU, RAM) of existing server
- Simpler implementation
- Limited by hardware
- Downtime during scaling

---

## Monitoring & Logging

### Q32: What is the difference between metrics, logs, and traces?
**Answer**:

**Metrics**: Numerical measurements over time
- CPU usage, memory, request rate
- Tools: Prometheus, CloudWatch

**Logs**: Timestamped records of events
- Application logs, error logs
- Tools: ELK Stack, Splunk

**Traces**: End-to-end request flow through distributed system
- Performance bottlenecks, latency
- Tools: Jaeger, Zipkin

**Together**: The "Three Pillars of Observability"

### Q33: How does Prometheus work?
**Answer**: Prometheus is a time-series database and monitoring system.

**Architecture**:
1. **Scraping**: Pulls metrics from targets via HTTP
2. **Storage**: Stores in time-series database
3. **Querying**: PromQL for querying data
4. **Alerting**: Alertmanager for notifications

**Key concepts**:
- **Targets**: Services to monitor
- **Exporters**: Expose metrics (node_exporter, blackbox_exporter)
- **Service Discovery**: Auto-discover targets
- **Labels**: Key-value pairs for dimensionality

---

## Security

### Q34: What is the principle of least privilege?
**Answer**: Grant only minimum permissions necessary to perform a task.

**Application**:
- IAM roles with specific permissions
- Service accounts for containers
- No root access unless required
- Regular permission audits

### Q35: How do you secure a Kubernetes cluster?
**Answer**:
1. **RBAC**: Role-Based Access Control
2. **Network Policies**: Control pod-to-pod communication
3. **Pod Security Policies/Standards**: Restrict pod capabilities
4. **Secrets Management**: Encrypt secrets at rest
5. **Image Scanning**: Scan for vulnerabilities
6. **Update regularly**: Keep K8s and components updated
7. **Audit logging**: Enable audit logs
8. **Limit API access**: Secure API server

### Q36: What is SAST vs DAST?
**Answer**:

**SAST (Static Application Security Testing)**:
- Analyzes source code without executing
- Finds vulnerabilities early
- Tools: SonarQube, Snyk, Checkmarx

**DAST (Dynamic Application Security Testing)**:
- Tests running application
- Simulates attacks
- Tools: OWASP ZAP, Burp Suite

**Use both** for comprehensive security!

---

## Behavioral Questions

### Q37: Describe a time when a deployment went wrong. How did you handle it?
**Template Answer**:
1. **Situation**: Describe the incident
2. **Task**: Your responsibility
3. **Action**: Steps you took (rollback, investigation, fix)
4. **Result**: Resolution and lessons learned

**Example**: "During a production deployment, we noticed a 500% increase in error rates..."
- Immediately rolled back using blue-green deployment
- Analyzed logs and found database migration issue
- Fixed migration script and tested in staging
- Implemented better pre-deployment checks
- Added automated smoke tests

### Q38: How do you stay updated with DevOps trends?
**Answer**:
- Follow DevOps blogs and newsletters
- Participate in communities (Reddit, Discord)
- Attend conferences and meetups
- Hands-on projects and labs
- Read documentation and release notes
- Follow thought leaders on Twitter/LinkedIn

---

## System Design

### Q39: Design a CI/CD pipeline for a microservices application
**Answer**:

**Components**:
1. **Source Control**: Git (GitHub/GitLab)
2. **CI Server**: GitHub Actions/Jenkins
3. **Build**: Docker images
4. **Testing**: Unit, integration, E2E
5. **Security**: SAST, image scanning
6. **Registry**: Docker Hub/ECR
7. **Deployment**: Kubernetes with Helm
8. **Monitoring**: Prometheus + Grafana

**Pipeline**:
```
Code Push → Build → Unit Tests → Integration Tests → 
Security Scan → Build Image → Push to Registry → 
Deploy to Staging → E2E Tests → Deploy to Prod (Canary) → Monitor
```

### Q40: How would you design a highly available web application?
**Answer**:

**Architecture**:
1. **Load Balancer**: Distribute traffic (ALB/NLB)
2. **Auto Scaling**: Scale based on demand
3. **Multiple AZs**: Deploy across availability zones
4. **Database**: Multi-AZ RDS or managed database
5. **Caching**: Redis/ElastiCache
6. **CDN**: CloudFront for static assets
7. **Backup**: Automated backups and DR plan
8. **Monitoring**: CloudWatch/Prometheus with alerts

**Key considerations**:
- No single point of failure
- Stateless application servers
- Data replication
- Health checks
- Graceful degradation

---

## Interview Tips

### Before the Interview:
1. **Research the company** - Understand their tech stack
2. **Review job description** - Match your experience
3. **Prepare examples** - STAR method for behavioral questions
4. **Practice coding** - If technical assessment is required
5. **Prepare questions** - Ask about team, processes, tools

### During the Interview:
1. **Clarify questions** - Don't assume, ask for clarification
2. **Think aloud** - Explain your reasoning
3. **Start simple** - Then optimize
4. **Ask questions** - Show engagement and curiosity
5. **Be honest** - If you don't know, say so and explain how you'd learn

### Common Question Patterns:
- "Tell me about a time when..." (Behavioral)
- "How would you design..." (System design)
- "What is the difference between..." (Conceptual)
- "How do you troubleshoot..." (Problem-solving)

---

## Additional Resources

- [DevOps Interview Questions - GitHub](https://github.com/bregman-arie/devops-exercises)
- [Kubernetes Interview Questions](https://github.com/iam-veeramalla/kubernetes-interview-questions)
- [System Design Primer](https://github.com/donnemartin/system-design-primer)

---

**Good luck with your interviews! 🚀**
