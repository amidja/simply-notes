
## Introduction 

[Skupper v2  Introduction](https://skupper.io/v2/index.html)

V2 has a new, uniform API for site configuration, site linking, and service exposure. In v2, all of Skupper's interfaces and platforms use this common API.
The following are the key API resources in v2:

| Sites and networks | [Site](https://skupperproject.github.io/refdog/resources/site.html), [Link](https://skupperproject.github.io/refdog/resources/link.html)                   |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Service exposure   | [Connector](https://skupperproject.github.io/refdog/resources/connector.html), [Listener](https://skupperproject.github.io/refdog/resources/listener.html) |

The latest release version as at 01/06/2026 is available from this [URL](https://github.com/skupperproject/skupper/releases/tag/2.1.3)

### Installation Notes

#### Install Skupper 2.0.0 at **cluster** scope (the operator pod runs in the **skupper** namespace):
```bash
$ kubectl create -f skupper-cluster-scope.yaml
$ kubectl get crd | grep skupper
$ kubectl get pods -n skupper
NAME                                  READY   STATUS    RESTARTS   AGE
skupper-controller-5f6b758c99-zwzlz   1/1     Running   0          3m45s

$ kubectl logs skupper-controller-5f6b758c99-zwzlz  -n skupper		

$ kubectl delete -f skupper-cluster-scope.yaml
$ kubectl get crd -o name | grep skupper | xargs kubectl delate
```

#### Install Skupper 2.0.0 at **namespace** scope (the operator pod runs in the **skupper** namespace):

The first thing we need to do is to create a signing CA that will be used between the namespaces:
```bash
mkdir ~/certs
cd ~/certs

#create the key first
openssl genrsa -des3 -out myCA.key 2048

#use the key to create the cert
openssl req -x509 -new -nodes -key myCA.key -sha256 -days 1825 -out myCA.pem
```

The first thing we need to do is to create a common secret containing the signing CA
To create a TLS Secret using `kubectl`, use the `tls` subcommand:
```shell 
$ kubectl create secret tls my-tls-secret --cert=certs/myCA.pem --key=certs/myCA-Decrypted.key -n west
$ kubectl create secret tls my-tls-secret --cert=certs/myCA.pem --key=certs/myCA.key -n east
```

```bash
#
# Deploy application in two namespaces
$ kubectl create namespace west
$ kubectl create deployment frontend --image quay.io/skupper/hello-world-frontend -n west
#
$ kubectl creat namespace east
$ kubectl create deployment backend --image quay.io/skupper/hello-world-backend --replicas 3 -n east
#
# Install  Skupper controller in each namespace
$ kubectl create -f skupper-namespace-scope.yaml -n west
$ kubectl create -f skupper-namespace-scope.yaml -n east
#
# Create East and West Skupper Sites (creates router) 
$ kubectl apply -n west -f site-west.yaml
$ kubectl apply -n east -f site-east.yaml
#
#Expose backend in east site
$ kubectl apply -n east -f connector-east.yaml
#
#Consume backend on west site
$ kubectl apply -n west -f listner-west.yaml
```

Link the East and West sites 
```bash
$ kubectl apply -n west -f access_grant.yaml
#
$ kubectl wait --for=condition=ready accessgrant/my-skupper-grant -n west && kubectl get accessgrant my-skupper-grant -n west -o yaml
#Copy ca, code and url fields from grant status into the spec section of an accesstoken (see access_token.yaml), and apply that in site east
$ kubectl apply -n east -f access_token.yaml
accesstoken.skupper.io/my-skupper-grant created

$ kubectl get accesstoken.skupper.io/my-skupper-grant -n east
```

The controller can also be installed using a skupper chart that can be found at this [URL](https://github.com/skupperproject/skupper/tree/main/charts)
This chart installs the [Skupper](https://skupper.io) version 2 controller for [Kubernetes](https://kubernetes.io) using the [Helm](https://helm.sh) package manager.

For more information on the chart please see these notes: [[skupper-v2-chart]]
## Creating a Skupper Site

https://docs.redhat.com/en/documentation/red_hat_service_interconnect/2.0/html/using_service_interconnect/kube-yaml
## Linking Skupper Sites

###  Hello World Example

Based from : 
- https://github.com/skupperproject/skupper/blob/main/cmd/controller/example/README.md
- https://github.com/skupperproject/skupper-example-hello-world/tree/main

Create test namespaces 'east' and 'west'

#### Test connectivity

```bash
kubectl -n west port-forward deployment/frontend 8080:8080
```

Visit http://localhost:8080

## References

[Skupper RefDog](https://skupperproject.github.io/refdog/index.html)

