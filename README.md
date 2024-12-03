# ArgoCD Applications

---
This repository contains ArgoCD ApplicationSet configurations for deploying various applications across different environments.  

## Repository Structure

---
`applications/create-namespaces.yaml`: Defines an ApplicationSet for creating namespaces in different environments.

`applications/nginx-app-1.yaml`: Defines an ApplicationSet for deploying nginx-app-1 in different environments.

`applications/nginx-app-2.yaml`: Defines an ApplicationSet for deploying nginx-app-2 in different environments.

## Prerequisites

---
✅ ArgoCD installed and configured in your Kubernetes cluster. 

✅ Access to the Kubernetes cluster where the applications will be deployed.
Usage

## Example

---
Creating namespaces, the create-namespaces.yaml file is used to create namespaces in different environments.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: create-namespace-applicationset
  namespace: argocd
spec:
  generators:
    - list:
        elements:
          - environment: dev
            server_url: https://kubernetes.default.svc
  template:
    metadata:
      name: 'create-namespace-{{environment}}'
    spec:
      project: default
      source:
        repoURL: 'https://github.com/christoffer-nielsen-knightec/argocd-applications'
        targetRevision: kustomize
        path: 'namespace/overlays/{{environment}}'
      destination:
        server: '{{server_url}}'
      syncPolicy:
        automated:
          prune: true
          selfHeal: true
```

## Modifications

---
Adding New Environments 

To add a new environment, update the generators section in each ApplicationSet file with the new environment details.  

Example for nginx-app-1.yaml:

```yaml
generators:
  - list:
      elements:
        - environment: dev
          server_url: https://kubernetes.default.svc
        - environment: qa
          server_url: https://kubernetes-qa.example.com
        - environment: stage
          server_url: https://kubernetes-stage.example.com
        - environment: prod
          server_url: https://kubernetes-prod.example.com
```