
## Introduction 

https://operator-framework.github.io/operator-controller/

What is an Operator subscription?

An operator subscription in Kubernetes (via Operator Lifecycle Manager - OLM) is a custom resource that tells the cluster which operator (like a database or monitoring tool) to install from a catalog, what version (channel), and how to manage updates (auto/manual). It acts as your "order" for an operator, ensuring OLM automatically installs and upgrades it, keeping your cluster's software current and managed, for more information refer to [this web page](https://olm.operatorframework.io/docs/concepts/crds/subscription/)


A sample yaml file looks like this:

```yml
apiVersion: operators.coreos.com/v1alpha1
kind: Subscription
metadata:
  name: my-skupper-operator
  namespace: operators
spec:
  channel: stable
  name: skupper-operator
  source: operatorhubio-catalog
  sourceNamespace: olm
```
## Install OLM v0 Framework on K3s

Install the latest version of the legacy version of the operator framework from [the projects' github.com repository](https://github.com/operator-framework/operator-lifecycle-manager?tab=readme-ov-file).

```bash

# Install Instructions
curl -L https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.38.0/install.sh -o install.sh
chmod +x install.sh
	./install.sh v0.38.0

# Source - https://stackoverflow.com/a
# Posted by Josiah
# Retrieved 2025-12-11, License - CC BY-SA 4.0

#first find the name of an olm-operator pod
kubectl get pods -n olm
kubectl exec -n olm olm-operator-5794f45f89-xv4jt -- olm --version
#OLM version: v0.38.0
#git commit: 6838e5e8be6bf78ea11bcc3e7b2964a29df0664e
	
#make sure it is working	
kubectl get packagemanifest -n olm 

```

## Install Skupper Operator
https://github.com/skupperproject/skupper-operator

```bash 
#This Operator will be installed in the "operators" namespace and will be usable from all namespaces in the cluster.

curl -O https://operatorhub.io/install/skupper-operator.yaml

kubectl create -f skupper-operator.yaml

kubectl -n operators get subscriptions
kubectl -n operators get installplans
```
### Skupper Cluster Deployment

[Skupper Site Controller](https://github.com/skupperproject/skupper/blob/1.9.4/cmd/site-controller/README.md)

A simple cluster config yaml:

```yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: skupper-site
data:
  #Global Site Config
  name: my-skupper-site
  cluster-local: "false"
  #Router Configuarion
  router-loggin: "info"
  #Console Configuration
  console: "true"
  console-authentication: internal
  console-password: "barney"
  console-user: "rubble"
  edge: "false"
  router-console: "true"
  service-controller: "true"
  service-sync: "true"
```


### Skupper Cluster Clean-up
https://olm.operatorframework.io/docs/tasks/uninstall-operator/

```bash
# delete the cluster first 
$kubectl delete -f skupper-site-cm.yaml

#delete the subscription
$kubectl delete -f skupper-operator.yaml
$kubectl get subscription -n operators

# Delete the Operator’s ClusterServiceVersion
$kubectl get clusterserviceversion -n operators
NAME                      DISPLAY   VERSION   REPLACES                  PHASE
skupper-operator.v1.9.4   Skupper   1.9.4     skupper-operator.v1.9.3   Succeeded

$kubectl delete clusterserviceversion skupper-operator.v1.9.4 -n operators
```

```
 
```
##  OpenShift OLM

https://www.redhat.com/en/blog/announcing-olm-v1-next-generation-operator-lifecycle-management


## Operator Bundle

https://olm.operatorframework.io/docs/tasks/creating-operator-bundle/

## References

- [Operator Framework GitHub Repository](https://github.com/operator-framework/)
- [Operator Framework, OLM's GitHub Repository](https://github.com/operator-framework/operator-lifecycle-manager?tab=readme-ov-file)
