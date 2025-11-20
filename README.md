# FluxLocalDev
Git Repo for use with FluxCD for local Kubernetes environment setup

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

# Bootstrap Flux to GitHub repo
flux bootstrap github \
  --owner=busadave13 \
  --repository=FluxLocalDev \
  --branch=dev \
  --path=./clusters/dev \
  --private-key-file=$env:USERPROFILE\.ssh\flux-local-dev-deploy-key

# Watch Flux reconcile
flux get kustomizations --watch
```