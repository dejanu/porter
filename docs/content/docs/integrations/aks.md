---
title: Connect to AKS
description: How to connect to an AKS cluster inside a Porter bundle.
weight: 1
---

## Connecting to an already existing AKS cluster

Prerequisites: az client CLI installed, guide [here](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)

1) Set the desired subscription:
```bash
az account set --subscription <subscription_ID>
```

2) Get the credentials for AKS:

* By default the credentials are merged in the current kubeconfig `~/.kube/config`, more about flags and usage [here](https://learn.microsoft.com/en-us/cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials)
```bash
az aks get-credentials --resource-group <reseource_group> --name <cluster_name> -o yaml
```
3) Check/set the Kubernetes context (most probably now you have multiple contexts stored in your kubeconfig file)
```bash
# list contexts
kubectl config get-contexts

# set context (the one that was AKS)
kubectl config use <AKS_context>
```

3) Define credentials in the `porter.yaml` for the kubeconfig:
```yaml
credentials:
  - name: kubeconfig
    path: /home/nonroot/.kube/config
```

4) Generate credentials:
```bash
porter credentials generate kubeconfig
```

## Create an AKS cluster with Porter

1) Install az-mixin: [docs](https://porter.sh/mixins/az/) and [repo](https://github.com/getporter/az-mixin)
```bash
porter mixin install az
```
2) Create porter bundle i.e. `demo_aks`
```bash
porter create demo_aks
```
3) Update `porter.yaml`
```yaml
mixins:
- az
```