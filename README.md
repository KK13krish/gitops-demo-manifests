# gitops-demo-manifests

Kubernetes manifests for the `hello-service` microservice — the GitOps source of truth.

Argo CD watches this repository and automatically syncs any changes to the cluster.

## Structure

```
k8s/
├── namespace.yaml      # gitops-demo namespace
├── deployment.yaml     # 2-replica Deployment (image tag updated by CI)
├── service.yaml        # ClusterIP service (port 80 → 5000)
└── kustomization.yaml  # Kustomize root
```

## How the image tag gets updated

The CI workflow in `gitops-demo-app` runs `sed` on `k8s/deployment.yaml` to replace
the `image:` line with the new `image:tag` after every push to `main`, then commits and
pushes back to this repo. Argo CD detects the change and rolls out the new version.

## Manual Sync (if needed)

```bash
# Using argocd CLI
argocd app sync hello-service

# Or via kubectl
kubectl get application hello-service -n argocd
```
