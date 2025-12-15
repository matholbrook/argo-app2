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

## How to View App Sync Settings

### With a Manual Sync Policy
```bash
$ argocd app get argocd/argo-app2 -o json | jq -r '.spec.syncPolicy'
null
```
Sync Policy:        Manual

### With an Automated Sync (no heal or prune)
```bash
$ argocd app set argocd/argo-app2 --sync-policy auto
$ argocd app get argocd/argo-app2 -o json | jq -r '.spec.syncPolicy'
{
  "automated": {}
}
```
Sync Policy:        Automated


### With an Automated Sync and Self-Heal (no prune)
```bash
$ argocd app set argocd/argo-app2 --sync-policy auto --self-heal
$ argocd app get argocd/argo-app2 -o json | jq -r '.spec.syncPolicy'
{
  "automated": {
    "selfHeal": true
  }
}
```

### Set Fully Automated Syncing
```bash
argocd app set argocd/argo-app2 --sync-policy automated --auto-prune --self-heal
$ argocd app get argocd/argo-app2 -o json | jq -r '.spec.syncPolicy'
{
  "automated": {
    "prune": true,
    "selfHeal": true
  }
}
```
Sync Policy:        Automated (Prune)


## Sync Retry Policies

```bash
$ argocd app set argocd/argo-app2 --sync-retry-limit 5
$ argocd app get argocd/argo-app2 -o json | jq -r '.spec.syncPolicy.retry'
{
  "limit": 5,
  "backoff": {
    "duration": "5s",
    "factor": 2,
    "maxDuration": "3m0s"
  }
}
```

$ argocd app set argocd/argo-app2 --sync-retry-limit 5 --sync-retry-backoff-duration 1m
