# Helm + ArgoCD Multi-Environment Setup

## File Structure
```
├── app-dev.yaml        # ArgoCD App for Development
├── app-qa.yaml         # ArgoCD App for QA
├── app-pro.yaml        # ArgoCD App for Production
└── my-app/
    ├── Chart.yaml
    ├── .helmignore
    ├── values.yaml         # Base values (nginx)
    ├── values-dev.yml      # Dev overrides  (replicas: 1)
    ├── values-qa.yml       # QA overrides   (replicas: 2)
    ├── values-pro.yml      # Prod overrides  (replicas: 5)
    └── templates/
        ├── deployment.yaml
        └── service.yaml
```

## Environments

| Env  | Namespace   | Replicas | Image Tag |
|------|-------------|----------|-----------|
| dev  | my-app-dev  | 1        | latest    |
| qa   | my-app-qa   | 2        | stable    |
| prod | my-app-pro  | 5        | stable    |

## Deploy

```bash
# Apply all environments
kubectl apply -f app-dev.yaml
kubectl apply -f app-qa.yaml
kubectl apply -f app-pro.yaml

# Check status
kubectl get applications -n argocd

# Check pods per env
kubectl get pods -n my-app-dev
kubectl get pods -n my-app-qa
kubectl get pods -n my-app-pro
```

## Later - Replace nginx image
Update `values.yaml`:
```yaml
image:
  repository: your-image
  tag: your-tag
```
Then push to GitHub — ArgoCD will auto-sync.
