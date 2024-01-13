# unfork-with-argocd

This repo is an article companion for [alezkv.pro/blog/unfork-with-argocd/](https://alezkv.pro/blog/unfork-with-argocd/)

## Setup local environment

Create king cluster:
```
kind create cluster --name unfork-with-argocd
```

Install ArgoCD:
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Configure ArgoCD to enable chart inflation for kustomize:
```
kubectl -n argocd patch configmaps argocd-cm -p '{"data":{"kustomize.buildOptions":"--enable-helm"}}'
```

Create root application:
```
kubectl apply -f root.yaml
```

## Check ArgoCD UI

Copy password into clipboard:
```
echo $(k -n argocd get secret argocd-initial-admin-secret --template={{.data.password}} | base64 -d) | pbcopy
```

Start port forwarding in background for ArgoCD web UI:
```
kubectl -n argocd port-forward svc/argocd-server 8888:80 >/dev/null 2>&1 &
```

Open browser and accept security warning:
```
open https://localhost:8888
```

It could be required sometimes to hit refresh on the root application.

## Clean up

Delete kind cluster:
```
kind delete cluster --name unfork-with-argocd
```
