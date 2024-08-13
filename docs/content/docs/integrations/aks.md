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
az aks get-credentials --resource-group <resource_group> --name <cluster_name> -o yaml
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

1) To access resources that are secured by a Microsoft Entra tenant, the entity that requires access must be represented by a security principal. This requirement is true for both users (user principal) and applications (service principal).

* A service principal is the local representation of an application object in a single Microsoft Entra tenant.
Furthermore we're going to use client secret for authethicating using service principals.


2) Install az-mixin: [docs](https://porter.sh/mixins/az/) and [repo](https://github.com/getporter/az-mixin)
```bash
porter mixin install az
```
3) Create porter bundle i.e. `demo_aks`
```bash
porter create demo_aks
```
4) Update `porter.yaml`
```yaml
mixins:
- az
```