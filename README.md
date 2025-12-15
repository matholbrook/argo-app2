# argo-app2

argocd testing

## Create Argo App using CLI

```bash
$ argocd app create argo-app2 \
--project default \
--repo https://github.com/matholbrook/argo-app2.git \
--path "./web-apache-2" \
--dest-server https://kubernetes.default.svc$
application 'argo-app2' created
```