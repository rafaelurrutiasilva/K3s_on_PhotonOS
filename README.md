[🇬🇧 English](README.md) / [🇸🇪 Svenska](README_se.md) 

# Running K3s on Photon OS
<img width="90" alt="K3s" src="https://landscape.cncf.io/logos/6fc4d4394f933b66196684183a6d138a3a8fc83d8d4f711028a80cba81c10397.svg" align=left>
<img width="90" alt="PhotonOS" src="https://github.com/rafaelurrutiasilva/images/blob/main/logos/Photon_OS.png" align=left>

<br>
<br>
Photon OS, VMware’s lightweight Linux distribution, is well-suited for containerized workloads. Combined with K3s, a lightweight Kubernetes distribution by Rancher, it provides a minimal and efficient environment for running Kubernetes clusters on virtualized infrastructure. This guide covers the basic steps to install and run K3s on Photon OS.

## Goals
Provide a concise guide for installing a Kubernetes cluster consisting of one Control Plane and three Worker Nodes, using K3s on Photon OS.

## Status
In Progress – _Content will be updated and may change._

## Method
We will start by using the default Photon OS version 5 distribution, remove unnecessary components, update the system, and perform basic OS configuration.

## Environment
The following computer environment was utilized. For details regarding container image versions and other components, please refer to the respective sections in the application documentation available here.
```
ESXi version: 8.0.0
ESXi build number: 20513097
VMware Photon OS: Version 5.0
Virtual Machine: 1vCPU, 2GB vRAM, 16GB vDisk
```

## Basic Configuration of the Photon OS VM
Setting hostname, setting root’s password and marking it non-expiring, removing Docker, updating system packages, configuring firewall to allow ICMP and SSH.
```
hostnamectl hostname k3s-control01 # (or k3s-worker01 or k3s-worker02 ..)                                                             
passwd [the new root passwd]
chage -I -1 -m 0 -M 99999 -E -1 root
tdnf remove docker
tdnf update -y
iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
iptables-save > /etc/systemd/scripts/ip4sav
```

## Basic installation of K3s
The [Install Script](https://docs.k3s.io/quick-start#install-script) is convenient way to install it as a service on systemd. 
```
curl -sfL https://get.k3s.io | sh -
```
After running this installation:
* The K3s service will be configured to automatically.
* Additional utilities will be installed, including kubectl, crictl, ctr, k3s-killall.sh, and k3s-uninstall.sh
* A kubeconfig file will be written to /etc/rancher/k3s/k3s.yaml and the kubectl installed by K3s will automatically use it.

### Controll after running the installation script
* Check service is running
```
systemctl status k3s.service
```
* Check Kubernetes node
```
kubectl get node
```
