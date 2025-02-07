# GitOps - Crossplane & ArgoCD

The following repository contains a `kustomize` project to:

- Install `ArgoCD`
- Configure `ArgoCD` to support `Crossplane` by updating the `ConfigMap`
- Add two `ArgoCD` repos for `Crossplane` and `external-secrets` (ESO)

# Introduction

Here are the steps to following to get it running

## 1. Step 1: ArgoCD

Run the following command, where `<env>` is the target environment.

```bash
kubectl apply -k argocd/install/overlays/<env>
```

This command:

- Setups and install `ArgoCD`
- Adds `Crossplane` helm repository to `ArgoCD`
- Adds `external-secrets` html repository to `ArgoCD`

## Step 2: Private Repository

If you have a private repository, you need to inject the secret to add it, at least this one to bootstrap the ArgoCD.

```bash
kubectl create secret generic private-repo-main \
    --namespace argocd \
    --from-env-file=.env \
    --labels=argocd.argoproj.io/secret-type=repo-creds
```

# Content of .env file should be:

```
url=<repository-url>
username=<username>
password=<personal-access-token>
```
