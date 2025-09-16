# Rancher Cluster with k3s, Helm and Hyper-V Manage

[Title: Setting Up an On-Premises Rancher Clust er with k3s, Helm and Hyper-V Manager | by Saad Ullah Khan Warsi | Medium](https://medium.com/@saadullahkhanwarsi/title-setting-up-an-on-premise-k3s-cluster-with-rancher-helm-and-hyper-v-manager-cc888edb178c)

## Hyper-V Setup for VMs

**Step1: Create a Virtual Network with Hyper-V**

```PS
Get-Module -ListAvailable -Name Hyper-V
Import-Module -Name Hyper-V -RequiredVersion 2.0.0.0
$ethernet = Get-NetAdapter -Name "Ethernet"
New-VMSwitch -Name "virtual-network" -NetAdapterName $ethernet.Name -AllowManagementOS $true -Notes "shared virtual network interface"

```

If the network adapter has the protocol used by the Hyper-V virtual switch still bound to it.
This is called the vms_pp binding. (Microsoft Virtual Network Switch Protocol) The network bindings needs to be modified following procedure outlined at this [url](https://learn.microsoft.com/en-us/troubleshoot/windows-server/virtualization/creating-v-switches-hyper-v-environment-fails)

These are the key commands:
```PS
Get-NetAdapterBinding -ComponentID "vms_pp"
Disable-NetAdapterBinding -Name "<Adapter Name>" -ComponentID "vms_pp"
```

**Step 2: Creating Virtual Machines**

```PS
mkdir c:\temp\vms\linux-0\
mkdir c:\temp\vms\linux-1\

New-VHD -Path c:\temp\vms\linux-0\linux-0.vhdx -SizeBytes 50GB
New-VHD -Path c:\temp\vms\linux-1\linux-1.vhdx -SizeBytes 50GB

New-VM -Name "linux-0" -Generation 2 -MemoryStartupBytes 2048MB -SwitchName "virtual-network" -VHDPath "c:\temp\vms\linux-0\linux-0.vhdx" -Path "c:\temp\vms\linux-0\"
New-VM -Name "linux-1" -Generation 2 -MemoryStartupBytes 2048MB -SwitchName "virtual-network" -VHDPath "c:\temp\vms\linux-1\linux-1.vhdx" -Path "c:\temp\vms\linux-1\"
```

**Step 3: Setting Up DVD**

[https://ubuntu.com/download/server](https://ubuntu.com/download/server)
This command adds a DVD drive to the VM, allowing you to boot from the specified ISO image.
```
Set-VMDvdDrive -VMName "linux-0" -ControllerNumber 1 -Path "C:\temp\ubuntu-25.04-live-server-amd64.iso"
Set-VMDvdDrive -VMName "linux-1" -ControllerNumber 1 -Path "C:\temp\ubuntu-25.04-live-server-amd64.iso"
```

This command did not work, had to add DVD using Hyper-V Manager.
Also, had to disable ''Enable Secure Boot" option under Security tab.

**Step 4: Starting the VMs**

```
Start-VM -Name "linux-0"  
Start-VM -Name "linux-1"
```

**Step 5: Starting the VMs**

In this step, access each VM using Hyper-V, and complete the initial Ubuntu setup.

### Setup SSH on VMs 

```bash
sudo apt update  
sudo apt install -y nano net-tools openssh-server  
sudo systemctl enable ssh  
sudo ufw allow ssh  
sudo systemctl start ssh
```

### Install Docker/Kubectl/Helm on Rancher Server
 
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
#install k9s
curl -fsSL -o k9s.deb  https://github.com/derailed/k9s/releases/download/v0.50.9/k9s_linux_amd64.deb
sudo dpkg -i k9s.deb

```

### K3s Cluster Setup

```bash
#k3s install
curl -sfL https://get.k3s.io | INSTALL_K3S_VERSION="v1.33.4+k3s1" K3S_TOKEN=saad946@ sh -s server --docker --node-ip=192.168.0.79 --advertise-address=192.168.0.79 --cluster-init  
#
#kubectl setup
mkdir -p ~/.kube  
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
# config your k3s cluster auth
whoami
sudo chown amidja:amidja ~/.kube/config  
kubectl get no
```


### Installing Rancher Server (Was not able to get it working)

##### Prerequisites
 ###### Installing Cert-manager:
```
helm repo add jetstack https://charts.jetstack.io
helm repo update  
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.1/cert-manager.crds.yaml
helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --version v1.13.1   
kubectl get pods --namespace cert-manager  
  
# Now we need to perform one additional step here  
kubectl edit deployment metrics-server -n kube-system
   at args:  
      - --kubelet-insecure-tls
```
 

Installing Rancher Server:
```bash
helm repo add rancher-latest https://releases.rancher.com/server-charts/latest
kubectl create namespace cattle-system
  
helm install rancher rancher-latest/rancher --namespace cattle-system /  
--set hostname=rancher.server.com --set bootstrapPassword=saad946@ /
--set ingress.tls.source=letsEncrypt /
--set letsEncrypt.ingress.class=nginx 
```

echo https://rancher.server.com/dashboard/?setup=$(kubectl get secret --namespace cattle-system bootstrap-secret -o go-template='{{.data.bootstrapPassword|base64decode}}')

https://rancher.server.com/dashboard/?setup=saad946@
 
 https://192.168.0.79/dashboard/?setup=saad946@
 
 kubectl get secret  bootstrap-secret -oyaml -n  cattle-system  
 rancher-85cff9bffc-ks5sq
kubectl port-forward rrancher-85cff9bffc-ks5sq 80:80 443:443 -n  cattle-system

