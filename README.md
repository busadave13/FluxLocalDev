# FluxLocalDev
Git Repo for use with FluxCD for local Kubernetes environment setup

```bash
export GITHUB_TOKEN=<your-personal-access-token>

flux bootstrap github \
  --owner=busadave13 \
  --repository=FluxLocalDev \
  --branch=dev \
  --path=./clusters/docker-desktop \
  --personal \
  --token-auth
```