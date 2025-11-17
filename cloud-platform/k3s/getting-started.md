# K3S

## Installation

- [Installation Doc](https://docs.k3s.io/installation)
- [Ref. Instructions](https://www.digitalocean.com/community/tutorials/how-to-setup-k3s-kubernetes-cluster-on-ubuntu)

#### Install K3s

```bash
#Install
curl -sfL https://get.k3s.io | sh -

#Check the status of the k3s service:
systemctl status k3s

#Check the default K8s Objects:
sudo kubectl get all -n kube-system

```

#### Uninstalling K3s

```shell
#Stop all K3s containers and reset containerd state
sudo /usr/local/bin/k3s-killall.sh

sudo rm -rf /var/lib/rancher /etc/rancher/k3s /var/lib/longhorn/

sudo /usr/local/bin/k3s-uninstall.sh
```
## Cluster Access 

The `kubeconfig` file stored at `/etc/rancher/k3s/k3s.yaml` is used to configure access to the Kubernetes cluster. 

If you have installed upstream Kubernetes command line tools such as kubectl or helm you will need to configure them with the correct kubeconfig path. This can be done by either exporting the `KUBECONFIG` environment variable or by invoking the `--kubeconfig` command line flag. Refer to the examples below for details.


 ```bash

## Do not do it this way. Have a look at the next section.
# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml

#sudo kubectl get pods --all-namespaces
#sudo helm ls --all-namespaces

#OR

kubectl --kubeconfig /etc/rancher/k3s/k3s.yaml get pods --all-namespaces
helm --kubeconfig /etc/rancher/k3s/k3s.yaml ls --all-namespaces

```
### Do not change permissions of k3s.yaml

```bash

export KUBECONFIG=~/.kube/config
mkdir -p ~/.kube 2> /dev/null
sudo k3s kubectl config view --raw > "$KUBECONFIG"
chmod 600 "$KUBECONFIG"
whoami
sudo chown ssm-user:ssm-user ~/.kube/config  
kubectl get no

```
