# 2025-05-crossplane-v2

meetup crossplane v2 preview

## Setup crossplane v2 preview

```bash
# install crossplane v2 preview
helm install crossplane \
--repo https://charts.crossplane.io/preview \
--namespace crossplane-system \
--create-namespace crossplane \
--version v2.0.0-preview.1
```

## Install function-patch-and-transform

```bash
kubectl apply -f crossplane/v2/function.yaml
```

## Prerequisites

- [cnpg](https://cloudnative-pg.io/documentation/current/installation_upgrade/#installation)
- [valkey-operator](https://github.com/hyperspike/valkey-operator)

```bash
# install valkey-operator
LATEST=$(curl -s https://api.github.com/repos/hyperspike/valkey-operator/releases/latest | jq -cr .tag_name)
helm install valkey-operator --namespace valkey-operator-system --create-namespace oci://ghcr.io/hyperspike/valkey-operator --version ${LATEST}-chart
```

## Deploy

```bash
kubectl apply -f crossplane/v2
kubectl apply -f deploy.yaml
```

## Update with yq

```bash
yq -i '.spec.replicas = 3' deploy.yaml
yq -i '.spec.image = "nginx:1.25.4"' deploy.yaml
kubectl apply -f deploy.yaml
```
