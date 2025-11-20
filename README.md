# FluxLocalDev
Git Repo for use with FluxCD for local Kubernetes environment setup

```bash
# Verify your context
kubectl config current-context
# Should show: docker-desktop

# Check cluster info
kubectl cluster-info

# Set your GitHub personal access token
export GITHUB_TOKEN=<your-personal-access-token>

# Bootstrap Flux to your GitHub repository
flux bootstrap github \
  --owner=busadave13 \
  --repository=FluxLocalDev \
  --branch=dev \
  --path=./clusters/docker-desktop \
  --personal \
  --token-auth
```