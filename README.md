# GitOps - Crossplane & ArgoCD

The purpose of this repository is to demonstrate GitOps practices using Crossplane and ArgoCD. It provides a streamlined approach to managing cloud infrastructure and Kubernetes resources through GitOps principles.

## Prerequisites

- Kubernetes cluster
- `kubectl` CLI tool
- Access to a Git repository
- Basic understanding of GitOps, Crossplane, and ArgoCD

## Architecture Overview

This setup implements a GitOps workflow where:

- ArgoCD manages the deployment and configuration of Crossplane
- Crossplane handles cloud infrastructure provisioning
- External Secrets Operator (ESO) manages sensitive information

# Setup in a local Kubernetes

## Install Kind

```bash
# Install Kind
brew install kind

# Create a local Kubernetes cluster
kind create cluster --name gitops-01

# Verify the cluster is running
kubectl cluster-info --context kind-gitops-01
```

## 1. Setup the cluster

The following command sets up the cluster with `ArgoCD`, `Crossplane`, and `external-secrets`:

```bash
kubectl apply -k argocd/install/overlays/local
```

## 2. Bootstrap the project

The project will be bootstraped following the `app-of-apps` pattern. Make sure to wait until the `CRDs` are created

```bash
kubectl apply -f argocd/bootstrap-gitops.yaml
```

## 3. (Optional) Private repo

If the `git` repo is private, create an `ArgoCD` repo (I prefer to declare as a `template`)
