# ACM Demo

Currently a WIP, this repository contains a collection of demos to highlight various GitOps patterns with Advanced Cluster Manager (and Argo CD).

## Current Capabilities

* Management Cluster Bootstrapped by Argo CD
	- ACM Deployment/Configuration
	- Ansible Tower Deployment/Configuration
* ACM Deploying Tekton Pipelines
* ACM Deploying Application on Multiple Production Clusters
	- Ansible Integration

## Bootstrap Management Cluster

The management cluster is managed by Argo CD. To deploy Argo CD, we will be using the OpenShift GitOps Operator. Install the operator as follows:

```console
oc apply -k gitops/manifests/bootstrap/openshift-gitops-operator/base
```

The operator will automatically deploy Argo CD. Configure it (create project, RBAC, enable OpenShift SSO) by running:

```console
oc apply -k gitops/manifests/bootstrap/openshift-gitops/base
```

This demo requires the use of sensitive information like certificates, credentials, tokens, etc. They are stored and protected in this repository using Sealed Secrets. Deploy the Sealed Secrets controller by running:

```console
kustomize build gitops/manifests/bootstrap/sealed-secrets/base | oc apply -f -
```

We will be leveraging an "App of Apps" pattern with Argo CD, deploy the initial application as follows:

```console
oc apply -k gitops/manifests/clusters/acm-hub/argocd/apps/bootstrap/base
```
