# Microk8s Install

> So simple, that your grandma can do it. Git it a try.

## A few things
```
sudo systemctl status ufw.service
sudo systemctl status sshd.service
```
Baremetal Machine
```
Series	GTR5  
Ram Memory Installed Size	32GB  
Operating System Fedora Linux  
CPU Model	Ryzen 9 4.6GHz  
CPU Manufacturer AMD  
Human Interface Input	Mouse, Keyboard  
Hard Disk Description	HDD, 500GB Solid State Hard Drive 
Wifi 6.0  
```

OS
``
NAME="Fedora Linux"
VERSION="36 (Workstation Edition)"
ID=fedora
VERSION_ID=36
``

## Microk8s

Reset
```
sudo snap remove microk8s
snap info microk8s
```
<img width="504" alt="image" src="https://user-images.githubusercontent.com/993459/183301359-a73ed580-0a31-4bd5-b8c7-1f687817b4f7.png">


Install
```
sudo snap install microk8s --classic --channel=1.24/stable
```
```
sudo snap alias microk8s.kubectl kubectl
```
```
sudo usermod -a -G microk8s $USER
```
```
kubectl config view --raw > ~/.kube/config
```
```
sudo chown -f -R $USER ~/.kube
```
```
exit
```

Enable Add-Ons
```
microk8s enable storage dns ingress
```

---
# AWX Operator Install - The prefered way for K8S
https://github.com/ansible/awx-operator

Do THIS









