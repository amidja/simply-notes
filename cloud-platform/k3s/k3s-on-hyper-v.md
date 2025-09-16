# Rancher Cluster with k3s, Helm and Hyper-V Manage

[Title: Setting Up an On-Premises Rancher Clust er with k3s, Helm and Hyper-V Manager | by Saad Ullah Khan Warsi | Medium](https://medium.com/@saadullahkhanwarsi/title-setting-up-an-on-premise-k3s-cluster-with-rancher-helm-and-hyper-v-manager-cc888edb178c)

## ### Install Docker/Kubectl/Helm
 
```bash
#docker install
curl -sSL https://get.docker.com/ | sh  
sudo usermod -aG docker $(whoami)  
sudo service docker start  
```

```bash
#kubectl setup  
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"  
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"  
echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check  
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
```

```bash
#helm install
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
helm version
```

```bash
#helmfile install
curl -LO https://github.com/roboll/helmfile/releases/download/v0.135.0/helmfile_linux_amd64
mv helmfile_linux_amd64 helmfile
chmod 777 helmfile
sudo mv helmfile /usr/local/bin
helmfile --version
```

```bash
#install k9s
curl -fsSL -o k9s.deb  https://github.com/derailed/k9s/releases/download/v0.50.9/k9s_linux_amd64.deb
sudo dpkg -i k9s.deb
```

### K3s Cluster Setup

```bash
#k3s install
curl -sfL https://get.k3s.io | INSTALL_K3S_VERSION="v1.33.4+k3s1" K3S_TOKEN=saad946@ sh -s server --docker --node-ip=192.168.0.77 --advertise-address=192.168.0.77 --cluster-init  
#
#kubectl setup
mkdir -p ~/.kube  
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
# config your k3s cluster auth
whoami
sudo chown amidja:amidja ~/.kube/config  
kubectl get no
```
[K3S Server Config](https://docs.k3s.io/cli/server)
 
kubectl get secret  bootstrap-secret -oyaml -n  cattle-system  
rancher-85cff9bffc-ks5sq
kubectl port-forward rrancher-85cff9bffc-ks5sq 80:80 443:443 -n  cattle-system