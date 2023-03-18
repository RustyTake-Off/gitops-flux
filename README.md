# gitops-flux

[Mini Project] - GitOps with [Flux](https://fluxcd.io/flux/).

## Prerequisites

* GitHub Repository

* GitHub [Personal Access Token (PAT)](https://github.com/settings/tokens)

* Kubernetes Cluster ([Kind](https://kind.sigs.k8s.io/), [Minikube](https://minikube.sigs.k8s.io/), AKS or other)

```bash
kind create cluster --name=staging
```

```bash
kind create cluster --name=production
```

## Bootstraping Flux on the cluster to a GitHub repository

For installing Flux on the cluster use the bellow commands, which will also connect it to provided GitHub repository.

It will create required resources in the cluster and store them in GitHub repository.

Command for bootstraping flux to staging environment

```bash
flux bootstrap github \
--components-extra=image-reflector-controller,image-automation-controller \
--context=kind-staging \
--owner=gitHubUserNameHere \
--repository=gitops-flux \
--branch=main \
--path=clusters/staging \
--personal \
--token-auth
```

Bootstrap to production environment

```bash
flux bootstrap github \
--components-extra=image-reflector-controller,image-automation-controller \
--context=kind-production \
--owner=gitHubUserNameHere \
--repository=gitops-flux \
--branch=main \
--path=clusters/production \
--personal \
--token-auth
```

The bootstrap commands commit manifests in the **flux-system directory** from the **clusters/staging** and **clusters/production** so it can pull changes to the cluster.

Check if things were deployed and can be accessed.

Run bellow commands on both clusters.

```bash
flux get kustomizations --watch
```

```bash
kubectl -n ingress-nginx port-forward svc/ingress-nginx-controller 8080:80
```

```bash
curl -H "Host: podinfo.$environmentName" http://localhost:8080
```
