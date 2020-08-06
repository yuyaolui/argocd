CLI
```
argocd app create --name test \
--repo https://github.com/DheerajJoshi/argocd.git \
--dest-server https://kubernetes.default.svc \
--dest-namespace <any-namespace> --path kubernetes
```

YAML

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: test
  namespace: <any-namespace>
spec:
  project: <project-name>
  source:
    repoURL: https://github.com/DheerajJoshi/argocd.git
    targetRevision: HEAD
    path: services/example-app
  destination:
    server: https://kubernetes.default.svc
    namespace: <any-namespace>
```
