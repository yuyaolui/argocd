# argocd
https://github.com/DheerajJoshi/argocd

Steps to install ArgoCD on kubernetes cluster:

1. Create namespace for ArgoCd
```
kubectl create namespace argocd
```

2. Install ArgoCD server and components
```
kubectl -n argocd apply -f argo-cd-installation/install.yaml
```

Get details of pod running using this command

```
$ watch kubectl get pods -n argocd

Every 2.0s: kubectl get pods -n argocd                                                                                         ITSG002707-MAC: Thu Aug  6 17:26:36 2020

NAME                                            READY   STATUS    RESTARTS   AGE
argocd-application-controller-d99d77688-dlqg6   0/1     Running   0          29s
argocd-dex-server-6c865df778-mdbz2              1/1     Running   0          29s
argocd-redis-8c568b5db-59xw4                    1/1     Running   0          29s
argocd-repo-server-675bddcbb8-lj5d2             1/1     Running   0          29s
argocd-server-75877b6ffb-qwxkq                  0/1     Running   0          29s

```
3. Once all pods are in running state then access ArgoCD UI by port forwarding. You can also use load balancing but I don't prefer to open CI/CD tool
```
kubectl port-forward svc/argocd-server -n argocd 8888:443
```

Access the UI on browser
```
https://localhost:8888
```

4. Login into the UI using username and password
```
username: admin
password: echo $password
```

For password get the name of the argocd server which is default password for ArgoCD admin user

```
password=$(kubectl get pods -n argocd | grep argocd-server | cut -f 1 -d " ")
```

5. Change admin password using ArgoCD CLI
```
$ argocd login localhost:8080

WARNING: server certificate had error: x509: certificate signed by unknown authority. Proceed insecurely (y/n)? y
Username: admin
Password:
'admin' logged in successfully
Context 'localhost:8888' updated
```

List all account
```
argocd account list
```

Get admin account details
```
argocd account get admin
```

Update password for admin account
```
argocd account update-password --account admin --current-password $password --new-password admin1234
```

Steps to install ArgoCD Rollout Controller on kubernetes cluster:

1. Create namespace for
```
kubectl create namespace argo-rollouts
```

2. Install ArgoCD Rollout components
```
kubectl -n argo-rollouts apply -f argo-cd-installation/rollout/install.yaml
```

3. Create Project using ArgoCD CLI
```
argocd proj create ovo-teralite-development --description "Project for Teralite development work" --dest \*,\*
```
```
argocd proj create ovo-teralite-production --description "Project for Teralite production work" --dest \*,\*
```

## Deploy Canary app
```
kubectl apply -f applications-defination/canary-app/app.yaml
```



## Before access the cluster (Make sure you update your /etc/hosts with same information)
Canary deployment
```
kubectl port-forward svc/canary-app -n canary-app 9999:80
```

Blue Green Deployment
```
kubectl port-forward svc/blue-green-app -n blue-green-app 9090:80
```
