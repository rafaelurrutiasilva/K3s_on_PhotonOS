[🇬🇧 English](README.md) / [🇸🇪 Svenska](README_se.md) 

# Köra K3s på Photon OS
<img width="90" alt="K3s" src="https://landscape.cncf.io/logos/6fc4d4394f933b66196684183a6d138a3a8fc83d8d4f711028a80cba81c10397.svg" align=left>
<img width="90" alt="PhotonOS" src="https://github.com/rafaelurrutiasilva/images/blob/main/logos/Photon_OS.png" align=left>

Photon OS, VMwares lättviktiga Linux-distribution, är väl lämpad för containerbaserade arbetslaster. I kombination med K3s, en lättviktig Kubernetes-distribution från Rancher, erbjuder den en minimal och effektiv miljö för att köra Kubernetes-kluster på virtualiserad infrastruktur. Denna guide beskriver de grundläggande stegen för att installera och köra K3s på Photon OS.

## Mål
En kortfattad guide för att installera ett Kubernetes-kluster med en kontrollnod (Control Plane) och tre arbetarnoder (Worker Nodes), baserat på K3s och körande på Photon OS.

## Status
Under utveckling - _Innehållet uppdateras och kommer att ändras._

## Metod
Vi börjar med att använda standarddistributionen av Photon OS version 5, ta bort onödiga komponenter, uppdatera systemet och genomföra grundläggande konfiguration av operativsystemet.

## Miljö för denna uppsättning
Följande datormiljö användes. För detaljer om versioner av containeravbildningar och andra komponenter, se respektive avsnitt i applikationens dokumentation som finns tillgänglig här.
```
ESXi version: 8.0.0
ESXi build number: 20513097
VMware Photon OS: Version 5.0
Virtual Machine: 1vCPU, 2GB vRAM, 16GB vDisk
```

## Grundläggande konfiguration av Photon OS
Sätta namn på noden, sätt roots passwd och att den aldrig går ut, ta bort docker, köra systemuppdatering, konfigurera brandvägger för ICMP och SSH.
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
