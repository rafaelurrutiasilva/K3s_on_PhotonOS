# Running K3s on Photon OS

<img width="90" alt="MyLogo" src="https://landscape.cncf.io/logos/6fc4d4394f933b66196684183a6d138a3a8fc83d8d4f711028a80cba81c10397.svg" align=left>
<img width="300" alt="MyLogo" src="https://camo.githubusercontent.com/c3b195e8681e9f3591ca53deb95b021cd98ec95811ff705cde2427959615a9e0/687474703a2f2f73746f726167652e676f6f676c65617069732e636f6d2f70726f6a6563742d70686f746f6e2f766d772d6c6f676f2d70686f746f6e2e737667" align=left>

<br>
<br>
Photon OS, VMware’s lightweight Linux distribution, is well-suited for containerized workloads. Combined with K3s, a lightweight Kubernetes distribution by Rancher, it provides a minimal and efficient environment for running Kubernetes clusters on virtualized infrastructure. This guide covers the basic steps to install and run K3s on Photon OS.

## Environment
The following computer environment was utilized. For details regarding container image versions and other components, please refer to the respective sections in the application documentation available here.
```
ESXi version: 8.0.0
ESXi build number: 20513097
VMware Photon OS: Version 5.0
Virtual Machine: 1vCPU, 2GB vRAM, 16GB vDisk
```

## Basic Configuration of the Photon OS VM
Setting hostname, running update, installing sudo and creating the user labuser.
```
hostnamectl hostname k3s-control01 # (or k3s-worker01 or k3s-worker02 ..)                                                             
tdnf remove docker
tdnf update -y
iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

