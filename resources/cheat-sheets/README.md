# DevOps Cheat Sheets

Quick reference guides for commonly used DevOps tools and commands.

## 📂 Available Cheat Sheets

- [Git Commands](#git-commands)
- [Docker Commands](#docker-commands)
- [Kubernetes (kubectl)](#kubernetes-kubectl)
- [Linux Commands](#linux-commands)
- [Terraform](#terraform)
- [Ansible](#ansible)

---

## Git Commands

### Basic Operations
```bash
# Initialize repository
git init

# Clone repository
git clone <url>

# Check status
git status

# Add files
git add <file>
git add .  # Add all files

# Commit changes
git commit -m "commit message"

# Push to remote
git push origin <branch>

# Pull from remote
git pull origin <branch>
```

### Branching
```bash
# Create new branch
git branch <branch-name>

# Switch branch
git checkout <branch-name>

# Create and switch
git checkout -b <branch-name>

# List branches
git branch -a

# Delete branch
git branch -d <branch-name>

# Merge branch
git merge <branch-name>
```

### Advanced
```bash
# View commit history
git log --oneline --graph

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# Stash changes
git stash
git stash pop

# Cherry-pick commit
git cherry-pick <commit-hash>

# Rebase
git rebase <branch>
```

---

## Docker Commands

### Container Management
```bash
# Run container
docker run <image>
docker run -d <image>  # Detached mode
docker run -p 8080:80 <image>  # Port mapping
docker run --name mycontainer <image>

# List containers
docker ps  # Running
docker ps -a  # All

# Stop container
docker stop <container-id>

# Start container
docker start <container-id>

# Remove container
docker rm <container-id>

# View logs
docker logs <container-id>
docker logs -f <container-id>  # Follow

# Execute command in container
docker exec -it <container-id> bash
```

### Image Management
```bash
# List images
docker images

# Pull image
docker pull <image>:<tag>

# Build image
docker build -t <name>:<tag> .

# Push image
docker push <name>:<tag>

# Remove image
docker rmi <image-id>

# Tag image
docker tag <image-id> <name>:<tag>

# Image history
docker history <image>
```

### Docker Compose
```bash
# Start services
docker-compose up
docker-compose up -d  # Detached

# Stop services
docker-compose down

# View logs
docker-compose logs -f

# List services
docker-compose ps

# Rebuild services
docker-compose build

# Scale services
docker-compose up -d --scale web=3
```

### Cleanup
```bash
# Remove unused containers
docker container prune

# Remove unused images
docker image prune

# Remove unused volumes
docker volume prune

# Remove everything
docker system prune -a
```

---

## Kubernetes (kubectl)

### Cluster Info
```bash
# Cluster info
kubectl cluster-info

# View nodes
kubectl get nodes

# View namespaces
kubectl get namespaces
```

### Pod Management
```bash
# List pods
kubectl get pods
kubectl get pods -n <namespace>
kubectl get pods -A  # All namespaces

# Describe pod
kubectl describe pod <pod-name>

# View logs
kubectl logs <pod-name>
kubectl logs -f <pod-name>  # Follow
kubectl logs <pod-name> -c <container>  # Specific container

# Execute command
kubectl exec -it <pod-name> -- bash

# Delete pod
kubectl delete pod <pod-name>
```

### Deployments
```bash
# List deployments
kubectl get deployments

# Create deployment
kubectl create deployment <name> --image=<image>

# Scale deployment
kubectl scale deployment <name> --replicas=3

# Update image
kubectl set image deployment/<name> <container>=<new-image>

# Rollout status
kubectl rollout status deployment/<name>

# Rollout history
kubectl rollout history deployment/<name>

# Rollback
kubectl rollout undo deployment/<name>

# Delete deployment
kubectl delete deployment <name>
```

### Services
```bash
# List services
kubectl get services

# Expose deployment
kubectl expose deployment <name> --port=80 --type=LoadBalancer

# Describe service
kubectl describe service <name>

# Delete service
kubectl delete service <name>
```

### Apply/Delete Resources
```bash
# Apply configuration
kubectl apply -f <file.yaml>
kubectl apply -f <directory>/

# Delete resources
kubectl delete -f <file.yaml>

# Get resource YAML
kubectl get <resource> <name> -o yaml
```

### Debugging
```bash
# Describe resource
kubectl describe <resource> <name>

# Get events
kubectl get events

# Port forwarding
kubectl port-forward <pod-name> 8080:80

# Copy files
kubectl cp <pod>:/path/to/file ./local-file
```

---

## Linux Commands

### File Operations
```bash
# List files
ls -la

# Change directory
cd /path/to/dir

# Create directory
mkdir -p /path/to/dir

# Remove file/directory
rm file
rm -rf directory

# Copy
cp source destination
cp -r source-dir destination-dir

# Move/Rename
mv source destination

# View file
cat file
less file
head -n 10 file
tail -f file  # Follow
```

### File Permissions
```bash
# Change permissions
chmod 755 file
chmod +x script.sh

# Change owner
chown user:group file
chown -R user:group directory
```

### Process Management
```bash
# List processes
ps aux
ps aux | grep process-name

# System monitor
top
htop

# Kill process
kill <PID>
kill -9 <PID>  # Force kill
killall <process-name>
```

### System Info
```bash
# Disk usage
df -h
du -sh directory

# Memory usage
free -h

# System info
uname -a
lsb_release -a

# CPU info
lscpu

# Network interfaces
ip addr
ifconfig
```

### Networking
```bash
# Test connectivity
ping google.com

# DNS lookup
nslookup domain.com
dig domain.com

# Network connections
netstat -tulpn
ss -tulpn

# Download file
wget <url>
curl -O <url>

# SSH
ssh user@host
ssh -i key.pem user@host
```

### Text Processing
```bash
# Search in files
grep "pattern" file
grep -r "pattern" directory

# Find files
find /path -name "*.txt"
find /path -type f -mtime -7

# Stream editor
sed 's/old/new/g' file

# Text processing
awk '{print $1}' file

# Cut columns
cut -d',' -f1,3 file
```

---

## Terraform

### Basic Commands
```bash
# Initialize
terraform init

# Format code
terraform fmt

# Validate
terraform validate

# Plan changes
terraform plan
terraform plan -out=plan.tfplan

# Apply changes
terraform apply
terraform apply plan.tfplan
terraform apply -auto-approve

# Destroy resources
terraform destroy
terraform destroy -auto-approve

# Show current state
terraform show

# List resources
terraform state list

# Output values
terraform output
```

### Workspace Management
```bash
# List workspaces
terraform workspace list

# Create workspace
terraform workspace new <name>

# Switch workspace
terraform workspace select <name>

# Delete workspace
terraform workspace delete <name>
```

### State Management
```bash
# Refresh state
terraform refresh

# Import existing resource
terraform import <resource> <id>

# Remove resource from state
terraform state rm <resource>

# Move resource
terraform state mv <source> <destination>

# Pull state
terraform state pull

# Push state
terraform state push
```

---

## Ansible

### Ad-hoc Commands
```bash
# Ping hosts
ansible all -m ping

# Run command
ansible all -a "uptime"

# Copy file
ansible all -m copy -a "src=/local/file dest=/remote/file"

# Install package
ansible all -m apt -a "name=nginx state=present"

# Restart service
ansible all -m service -a "name=nginx state=restarted"
```

### Playbook Commands
```bash
# Run playbook
ansible-playbook playbook.yml

# Check syntax
ansible-playbook playbook.yml --syntax-check

# Dry run
ansible-playbook playbook.yml --check

# With specific inventory
ansible-playbook -i inventory.ini playbook.yml

# With extra variables
ansible-playbook playbook.yml -e "var=value"

# Limit to hosts
ansible-playbook playbook.yml --limit webservers

# With tags
ansible-playbook playbook.yml --tags "deploy"
```

### Inventory
```bash
# List hosts
ansible-inventory --list

# Show host variables
ansible-inventory --host <hostname>

# Graph inventory
ansible-inventory --graph
```

---

## Quick Tips

### Docker
- Use `.dockerignore` to exclude files from builds
- Always use specific tags, avoid `:latest`
- Use multi-stage builds to reduce image size
- Run containers as non-root user

### Kubernetes
- Use namespaces to organize resources
- Set resource limits (CPU/memory) for pods
- Use labels for better organization
- Keep manifests in version control

### Terraform
- Use remote state backend (S3, Azure Storage)
- Lock state file to prevent conflicts
- Use modules for reusability
- Keep secrets in separate files (.gitignore them)

### Security Best Practices
- Never commit secrets to Git
- Use secrets management (Vault, AWS Secrets Manager)
- Scan Docker images for vulnerabilities
- Keep systems updated
- Use principle of least privilege

---

**💡 Pro Tip**: Create your own cheat sheet as you learn. Write down commands you use frequently!
