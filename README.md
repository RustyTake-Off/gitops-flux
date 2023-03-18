# gitops-flux

[Mini Project] - GitOps with [Flux](https://fluxcd.io/flux/).

## Prerequisites

* GitHub Repository

* GitHub [Personal Access Token (PAT)](https://github.com/settings/tokens)

* Kubernetes Cluster ([Kind](https://kind.sigs.k8s.io/), [Minikube](https://minikube.sigs.k8s.io/), AKS or other)

## Bootstraping Flux on a cluster to a GitHub repository

For installing Flux on the cluster use the bellow command, which will also connect it to provided GitHub repository.

It will also create in it the required resources.

```bash
flux bootstrap github \
  --components-extra=image-reflector-controller,image-automation-controller \
  --owner=gitHubUserNameHere \
  --repository=gitops-flux \
  --branch=main \
  --path=clusters/demo-lab \
  --personal \
  --token-auth
```

## Add the deployment files
