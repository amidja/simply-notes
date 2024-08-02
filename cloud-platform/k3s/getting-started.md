# K3S

## Instalation

[Installation](https://docs.k3s.io/installation)
[Instractutions](https://www.digitalocean.com/community/tutorials/how-to-setup-k3s-kubernetes-cluster-on-ubuntu)
Install:
```bash
curl -sfL https://get.k3s.io | sh -

```

Check the status of the k3s service:
```bash 
systemctl status k3s

```
Check the default K8s Objects:
```bash
sudo kubectl get all -n kube-system
```

## Cluster Access 

The kubeconfig file stored at /etc/rancher/k3s/k3s.yaml is used to configure access to the Kubernetes cluster. 


 If you have installed upstream Kubernetes command line tools such as kubectl or helm you will need to configure them with the correct kubeconfig path. This can be done by either exporting the KUBECONFIG environment variable or by invoking the --kubeconfig command line flag. Refer to the examples below for details.

 ```bash
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
#kubectl get pods --all-namespaces
#helm ls --all-namespaces
 ```

Or 

```bash
kubectl --kubeconfig /etc/rancher/k3s/k3s.yaml get pods --all-namespaces
helm --kubeconfig /etc/rancher/k3s/k3s.yaml ls --all-namespaces
```

## UI Dashboard 

https://github.com/kubernetes/dashboard



```bash
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/

helm upgrade --install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --create-namespace --namespace kubernetes-dashboard
```

```text

To access Dashboard run:
  kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard-kong-proxy 8443:443

NOTE: In case port-forward command does not work, make sure that kong service name is correct.
      Check the services in Kubernetes Dashboard namespace using:
        kubectl -n kubernetes-dashboard get svc

Dashboard will be available at:
  https://localhost:8443

```


### Accessing Dashboard 

https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

```bash
kubectl -n kubernetes-dashboard create token admin-user
#or
kubectl get secret admin-user -n kubernetes-dashboard -o jsonpath={".data.token"} | base64 -d

```






