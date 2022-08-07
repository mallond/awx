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

# AWX Operator Install - The prefered way for K8S. 
Aug 7, 2022

This is the prefered way to install AWX. As AWX is a Kubernettes application.
[Basic Install Instructions](https://github.com/ansible/awx-operator#basic-install)

Install Kustomization
```
curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh"  | bash
```
Create Kustomization.yml
```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  # Find the latest tag here: https://github.com/ansible/awx-operator/releases
  - github.com/ansible/awx-operator/config/default?ref=0.26.0
  - awx.yml
# Set the image tags to match the git version from above
images:
  - name: quay.io/ansible/awx-operator
    newTag: 0.26.0
```
Execute Kustomization
```
./kustomize build . | kubectl apply -f -
namespace/awx
```
Set our Context
```
kubectl config set-context --current --namespace=awx
```
Verify
```
kubectl get pods -n awx

NAME                                               READY   STATUS    RESTARTS   AGE
awx-operator-controller-manager-7f89bd5797-q9s85   2/2     Running   0          2m39s
awx-beelink-postgres-13-0                          1/1     Running   0          2m5s
awx-beelink-b94f6dfbb-lr8jc                        4/4     Running   0          96s
```

Create AWX
```
---
apiVersion: awx.ansible.com/v1beta1
kind: AWX
metadata:
  name: awx-beelink
spec:
  service_type: nodeport
  # default nodeport_port is 30080
  nodeport_port: 30080
```













