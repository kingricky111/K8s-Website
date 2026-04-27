⸻

README.md

# Kubernetes Portfolio Website 
This project is my personal portfolio website which was deployed on azure and now is deployed on a self-managed Kubernetes cluster using a GitOps workflow with Argo CD.

---
##  Overview
- Static portfolio website containerized with Docker
- Deployed to Kubernetes using:
  - Deployment
  - Service
  - Ingress
- Managed via **Argo CD (GitOps)**
- Public DNS handled through **Cloudflare**
- HTTPS via **cert-manager (Let's Encrypt)**
- Cluster monitoring with **Prometheus & Grafana**
---
##  Architecture

User → Cloudflare DNS → Public WAN IP → Router Port Forwarding
→ Ingress Controller → Kubernetes Service → Pod (Nginx)

---
##  Tech Stack
- Kubernetes (kubeadm, bare metal)
- Docker
- Argo CD
- NGINX Ingress Controller
- cert-manager (Let's Encrypt)
- Cloudflare DNS
- Prometheus & Grafana
---
##  Repository Structure

k8s-website/
├── apps/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── namespace.yaml
│   └── kustomization.yaml
├── argocd/
│   └── applications/
│       └── k8s-website.yaml

---
##  Deployment Flow (GitOps)
1. Update application image
2. Modify `deployment.yaml` with new image tag
3. Push changes to GitHub
4. Argo CD detects change
5. Argo CD syncs cluster state
6. Kubernetes rolls out updated pods
---
##  DNS Configuration
Cloudflare DNS setup:

A     @     → PUBLIC_WAN_IP
CNAME www   → rickycloudresume.com

---
## TLS / HTTPS
- Managed using **cert-manager**
- Automatically provisions certificates via Let's Encrypt
- Ingress handles HTTPS (port 443) and routes internally to port 80
---
## Monitoring
Cluster-level monitoring is implemented using:
- Prometheus (metrics collection)
- Grafana (visualization dashboards)
Metrics include:
- CPU / memory usage
- Pod health and restarts
- Node performance
- Network traffic
---
##  Challenges & Lessons Learned
### 1. Argo CD Application Deployment Issue
- Incorrect API version (`argoproject.io` vs `argoproj.io`)
- Result: Application resource failed to apply
### 2. GitOps Path Mismatch
- Argo CD could not locate manifests due to incorrect repo path
- Fixed by aligning `path` with repository structure
### 3. DNS & Networking Confusion
- Initially attempted to use internal and incorrect IPs
### 4. Cloudflare 522 Error
- Root cause: no router port forwarding
- Fix: forward ports 80 and 443 to ingress controller
---
##  Future Improvements
- GitHub Actions CI/CD pipeline for automated image builds and deployments
- Application-level metrics (NGINX Prometheus exporter)
- Multi-environment setup (dev/prod overlays)
---
##  Live Site
[https://www.rickycloudresume.com](https://www.rickycloudresume.com)
---
## License
This project is for educational and portfolio purposes.

⸻

