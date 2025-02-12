
```markdown
# Kubernetes & Linux Sandbox Environment 🚀

A cloud-init configured Multipass instance for Kubernetes practice, Linux administration, and DevOps learning. Perfect for creating disposable sandbox environments.

[![Multipass](https://img.shields.io/badge/Multipass-v1.11+-blue?logo=canonical)](https://multipass.run)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-v1.28+-326CE5?logo=kubernetes)](https://kubernetes.io)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

## Features ✨

- **Pre-configured Kubernetes Cluster** (k3s lightweight)
- **Development Tools**: Docker, kubectl, Helm, Python, Git
- **Networking Utilities**: nmap, net-tools, dnsutils
- **Monitoring**: htop, ncdu, bashtop
- **Security**: fail2ban, UFW firewall rules
- **Automated Setup** via cloud-init
- **Cross-Platform** (Windows/macOS/Linux)

## Repository Structure 📂


.
├── cloud-config.yaml          # Main cloud-init configuration
├── README.md                 # This documentation
└── examples/                 # Sample deployments
    ├── nginx-deployment.yaml
    └── redis-statefulset.yaml
```

## Prerequisites 🛠️

- [Multipass](https://multipass.run) 1.11+
- 4 CPU cores available
- 8GB+ free RAM
- 40GB+ disk space
- Virtualization enabled in BIOS

## Quick Start 🚦

1. **Clone Repository**
```bash
git clone https://github.com/<your-username>/kubernetes-sandbox.git
cd kubernetes-sandbox
```

2. **Launch Instance** (PowerShell)
```powershell
multipass launch jammy -n k8s-lab `
  --cloud-init .\cloud-config.yaml `
  -c 4 -m 8G -d 40G `
  --mount ${HOME}\projects:/home/devuser/projects
```

For Unix/Linux systems:
```bash
multipass launch jammy -n k8s-lab \
  --cloud-init ./cloud-config.yaml \
  -c 4 -m 8G -d 40G \
  --mount $HOME/projects:/home/devuser/projects
```

## Cloud-Config Breakdown ⚙️ ([cloud-config.yaml](cloud-config.yaml))

Key sections:
```yaml
# Base Configuration
packages:
  - docker.io
  - kubectl
  - helm
  - python3-venv

# Kubernetes Setup
runcmd:
  - curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="--disable traefik" sh -
  
# Security Hardening
  - ufw allow 6443
  - fail2ban-client add sshd
```

## Common Use Cases 💡

### 1. Kubernetes Cluster Practice
```bash
# Deploy sample application
kubectl create deployment nginx --image=nginx:alpine

# Expose service
kubectl expose deployment nginx --port=80 --type=NodePort
```

### 2. Linux Administration
```bash
# Network troubleshooting
tcpdump -i eth0 port 80

# Process monitoring
htop
```

### 3. DevOps Workflows
```bash
# Build and push Docker images
docker build -t myapp:latest .
docker run -p 8080:80 myapp:latest
```

## Making it Perfect 🔧

### Recommended Customizations
1. **Add Persistent Storage**
```yaml
# In cloud-config.yaml
runcmd:
  - kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/master/deploy/longhorn.yaml
```

2. **Enable Monitoring Stack**
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install monitoring prometheus-community/kube-prometheus-stack
```

### Performance Tips
- Allocate more resources: `-c 8 -m 16G` for production-like clusters
- Use SSD storage: `-d 100G --disk ssd`
- Enable GPU passthrough: `--gpu`

## Troubleshooting 🔍

### Common Issues
1. **Mounts Disabled**
```powershell
# Enable Hyper-V mounts
multipass set local.mounts.enabled=true
```

2. **Cloud-Init Errors**
```bash
# View initialization logs
multipass exec k8s-lab -- tail -f /var/log/cloud-init-output.log
```

3. **Resource Constraints**
```bash
# Resize existing instance
multipass stop k8s-lab
multipass start k8s-lab --memory 16G --disk 80G
```

## Contributing 🤝

1. Fork the repository
2. Create feature branch (`git checkout -b feature/improvement`)
3. Commit changes (`git commit -am 'Add some feature'`)
4. Push to branch (`git push origin feature/improvement`)
5. Open Pull Request

## License 📄

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**Happy Kubernetting!** 🎉  
*Remember: This is a sandbox environment - always test in staging before production!*
