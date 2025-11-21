# Local Dev Environment
This repo is a FluxCD repository to deploy a local Kubernetes environment using Docker Desktop's built-in Kubernetes cluster.
Use the instructions below to bootstrap FluxCD to the local cluster and deploy applications such as Istio, Prometheus, Grafana, Kiali, and Aspire.

## Manual Setup Instructions

1. Update your local hosts file to map the application URLs to `127.0.0.1`
```bash
127.0.0.1 prometheus.tools.com
127.0.0.1 kiali.tools.com
127.0.0.1 grafana.tools.com
127.0.0.1 aspire.tools.com
```
2. Create a GitHub Personal Access Token (PAT) with repo permissions.
3. Create an SSH deploy key for FluxCD and add the public key to your GitHub repository.
```bash
ssh-keygen -f $env:USERPROFILE\.ssh\flux-local-dev-deploy-key
```
4. Use the `flux bootstrap github` command to bootstrap FluxCD to your local Docker Desktop
```bash
flux bootstrap github \
  --owner=busadave13 \
  --repository=FluxLocalDev \
  --branch=dev \
  --path=./clusters/dev \
  --private-key-file=$env:USERPROFILE\.ssh\flux-local-dev-deploy-key
```
5. Create a Slack webhook and create the Kubernetes secret for FluxCD alerts.
```bash
kubectl create secret generic slack-url \
  --from-literal=address=https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK \
  -n flux-system
```

## Debugging FluxCD locally

```bash
## Switch to Docker Desktop Kubernetes context
```bash
# Verify your context
kubectl config current-context
# Should show: docker-desktop

# Check cluster info
kubectl cluster-info

```

## Create SSH deploy key for Flux
```bash
cd $env:USERPROFILE\.ssh
ssh-keygen -t ed25519 -C "flux-home-lab" -f flux-home-lab-deploy-key

# Copy the PUBLIC key
Get-Content $env:USERPROFILE\.ssh\flux-home-lab-deploy-key.pub
```

## Bootstrap docker-desktop cluster
```bash

# Create a PAT on GitHub to bootstrap Flux

# Bootstrap Flux to GitHub repo
flux bootstrap github \
  --owner=busadave13 \
  --repository=FluxLocalDev \
  --branch=dev \
  --path=./clusters/dev \
  --private-key-file=$env:USERPROFILE\.ssh\flux-local-dev-deploy-key
```

## Flux Commands
```bash
# Check HelmRelease status
kubectl get helmrelease istio-ingress -n flux-system

# Reconcile specific HelmRelease
flux reconcile helmrelease istio-ingress -n flux-system

# Watch Flux reconcile
flux get kustomizations --watch

flux reconcile source git flux-system
flux reconcile kustomization flux-system
flux reconcile kustomization <kustomization-name> -n flux-system
```

## Istio Ingress Commands
```bash
kubectl get Gateway gateway-api -n istio-ingress
```