
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

New-VM -Name "linux-0" -Generation 2 -MemoryStartupBytes 4096MB -SwitchName "virtual-network" -VHDPath "c:\temp\vms\linux-0\linux-0.vhdx" -Path "c:\temp\vms\linux-0\"
New-VM -Name "linux-1" -Generation 2 -MemoryStartupBytes 4096MB -SwitchName "virtual-network" -VHDPath "c:\temp\vms\linux-1\linux-1.vhdx" -Path "c:\temp\vms\linux-1\"
```

**Step 3: Setting Up DVD**

[https://ubuntu.com/download/server](https://ubuntu.com/download/server)
This command adds a DVD drive to the VM, allowing you to boot from the specified ISO image.
```
Set-VMDvdDrive -VMName "linux-0" -ControllerNumber 1 -Path "C:\temp\ubuntu-25.04-live-server-amd64.iso"
Set-VMDvdDrive -VMName "linux-1" -ControllerNumber 1 -Path "C:\temp\ubuntu-25.04-live-server-amd64.iso"
```

**This command did not work, had to add DVD using Hyper-V Manager.**
**Also, had to disable ''Enable Secure Boot" option under Security tab.**

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
