# Tools

## Helm

Helm is a tool that automates the creation, packaging, configuration, and deployment of Kubernetes applications by combining your configuration files into a single reusable package.

### Installing Helm

[Helm Install ] (https://helm.sh/docs/intro/install/)
[Helm on GitHub](https://github.com/helm/helm)

#### Initialize a Helm Chart Repository

```text
 helm repo add bitnami https://charts.bitnami.com/bitnami
 helm search repo bitnami
```

### Helm Charts

A Helm chart is a package that contains all the necessary resources to deploy an application to a Kubernetes cluster. This includes YAML configuration files for deployments, services, secrets, and config maps that define the desired state of your application.

Helm charts allow you to manage Kubernetes manifests without using the Kubernetes command-line interface (CLI) or remembering complicated Kubernetes commands to control the cluster.

[Helm repository Artifact Hub](https://artifacthub.io/)
[Bitnami Charts](https://github.com/bitnami/charts/tree/main/bitnami)

#### Scaffolding a Helm chart

To scaffold a sample Helm chart that can customized to a specific needs. All what we need to do is run:

```text
helm create mychart
```

This will create a directory called 'mychart' that contains the necessary files to deploy a fully functional Helm chart. 

#### Working with a Helm chart

The the rendering of the template:

```text
helm template example-app example-app
```

Install the app using the chart:

```text
helm install example-app example-app

# list our releases

helm list

# see our deployed components

kubectl get all
kubectl get cm
kubectl get secret
```

Upgrade the release"

```text
# upgrade our release
helm upgrade example-app example-app --values ./example-app/values.yaml

# see revision increased
helm list
```

### Helm  Reference/Docs

[A Complete Guide - What is Helm](https://circleci.com/blog/what-is-helm/)
[Helm repository - Artifact Hub](https://artifacthub.io/)