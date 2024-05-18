# Walkthrough on ArgoCD App of App pattern

## Prerequisites

1. Existing Kubernetes cluster (Docker-Desktop, kind, minikube, etc)
2. Helm
3. Create Git token (optional - needed if clone/fork this to a private repo)

## Install ArgoCD to your k8s cluster

- `helm upgrade --install --wait --timeout 15m --atomic -n argocd --create-namespace argocd ./argo-cd`
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
- Ensure that the Connection Status is successful before proceeding to create new applications

## Deploy ArgoCD Applications

### Deploy staging environment to namespace `staging`

- kubectl -n argocd apply -f ./argocd-apps/parent/staging.yaml

### Deploy prod environment to namespace `prod`

- kubectl -n argocd apply -f ./argocd-apps/parent/prod.yaml

## Explanation of examples

### Staging

- 3 `directory` type apps are deployed
- `staging-guesbook-app3` has sync disabled
- Purpose is to showcase the parent is healthy even though the child is not in sync (parent is managing its child Applications, not the apps that the child Applications are managing)

### Prod

- 1 `directory` app, 1 `helm` app, 1 `kustomize` app
- Purpose is to showcase/provide template on how to manage different types of Applications
