# Ansible Yii2 App Deploy

## 🚀 Overview
Automated infrastructure setup and application deployment for Yii2 applications on AWS EC2 using Ansible. This playbook is designed to be executed via EC2 user data during instance launch [yii2-devops-stack](https://github.com/uddeshya307/yii2-devops-stack), providing complete infrastructure automation and Docker Swarm deployment.


## 🏗️ Architecture

The playbook sets up the following infrastructure:

```
┌─────────────────────────────────────────┐
│              AWS EC2 Instance           │
├─────────────────────────────────────────┤
│  ┌─────────────┐    ┌─────────────────┐ │
│  │   NGINX     │────│  Docker Swarm   │ │
│  │   :80/443   │    │                 │ │
│  └─────────────┘    │  ┌─────────────┐│ │
│         │            │  │ Yii2 App   ││ │
│         │            │  │ Container  ││ │
│         └────────────┼─►│   :9000    ││ │
│                      │  └─────────────┘│ │
│                      └─────────────────┘ │
└─────────────────────────────────────────┘
```



### Automatic Deployment (via Terraform)

This playbook is automatically executed when using the [yii2-devops-stack](https://github.com/uddeshya307/yii2-devops-stack) Terraform configuration:

```bash
# Clone the main DevOps stack
git clone https://github.com/uddeshya307/yii2-devops-stack.git
cd yii2-devops-stack/terraform_ec2

# Deploy infrastructure (includes automatic Ansible execution)
terraform apply
```

The Terraform user data script will:
1. Download this Ansible playbook
2. Execute `playbook.yml` automatically
3. Set up the complete infrastructure
4. Deploy your Yii2 application

### Manual Execution

For manual deployment or custom configurations:

```bash
# Clone this repository
git clone https://github.com/uddeshya307/ansible-yii2-app-deploy.git
cd ansible-yii2-app-deploy

# Run the complete playbook
ansible-playbook -i inventories/hosts.ini playbook.yml
```




### NGINX Configuration Template

The `yii2app.conf.j2` template provides:

- Reverse proxy to Docker Swarm service
- Static file serving


## 🚀 Deployment Process

When executed, the playbook follows this sequence:

1. **Docker Environment Setup**
   - Install Docker and Docker Compose
   - Start Docker service
   - Initialize Docker Swarm


2. **System Setup and Application Initialization**
   - Install system and application dependencies
   - Clone Git repo  [yii2-devops-stack](https://github.com/uddeshya307/yii2-devops-stack)


3. **Web Server Configuration**
   - Install and configure NGINX
   - Set up reverse proxy configuration

4. **Application Deployment**
   - Generate Docker Compose configuration
   - Deploy application as Swarm service




### Debug Mode

Enable verbose output for troubleshooting:

```bash
# Run with increased verbosity
ansible-playbook -i inventory/hosts playbook.yml -vvv

# Check specific task results
ansible-playbook -i inventory/hosts playbook.yml --start-at-task="Install Docker"
```

## 📚 Related Documentation

- **Main DevOps Stack**: [yii2-devops-stack](https://github.com/uddeshya307/yii2-devops-stack)

