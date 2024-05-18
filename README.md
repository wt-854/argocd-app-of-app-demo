# Walkthrough on ArgoCD App of App pattern

## Prerequisites

1. Existing Kubernetes cluster (Docker-Desktop, kind, minikube, etc)
2. Helm
3. Create Gitlab token (no permission in this repo, clone/fork this elsewhere)

## Install ArgoCD to your k8s cluster

- helm upgrade --install --wait --timeout 15m --atomic -n argocd --create-namespace argocd ./argo-cd
- To get password, run `kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo`
- To access UI, open another terminal and run `kubectl -n argocd port-forward svc/argocd-server 8080:80`

## Add your Git repo to ArgoCD

- Settings -> Repositoriies -> CONNECT REPO
- VIA HTTPS
- Type: git
- Project: default
- Repository URL (location of where you commit this to)
- Username (name of token)
- Password (token that you created)
- Skip server verification (tick this option)
- CONNECT (button at the top)
