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









-----

## Docker Install
```
docker run -d --name awx --restart=always -p 8043:8043 -p 8052:8052 quay.io/ansible/awx
```

# Install Kustomize
```
curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh"  | bash
```

# AWX Operator
vi kuxtomize.yml
```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  # Find the latest tag here: https://github.com/ansible/awx-operator/releases
  - github.com/ansible/awx-operator/config/default?ref=0.26.0

# Set the image tags to match the git version from above
images:
  - name: quay.io/ansible/awx-operator
    newTag: 0.26.0
```

# Specify a custom namespace in which to install AWX
namespace: awx

```
kubectl get pods -n awx
NAME                                               READY   STATUS    RESTARTS   AGE
awx-operator-controller-manager-7f89bd5797-fkpsp   2/2     Running   0          2m2s
```

```
./kustomize build . | kubectl apply -f -
namespace/awx
```


# AWX CLI
```
pip3 install awxkit
```











## AWX

> Important note: Please read install.md, as Google can waste your time if you get a wrong instructions

Setup AWX Operator
```
git clone https://github.com/ansible/awx-operator.git
cd awx-operator
```
Find the Branch 
[AWX Operator Release](https://github.com/ansible/awx-operator/releases)

Create namespace
```
kubectl create namespace ansible-awx
export NAMESPACE=ansible-awx
```
> Important: export NAMESPACE=ansible-awx To be used in the rest of these steps



Change your branch (what you selected), create namespace and make the deploy
```
git checkout 0.26.0
sudo make deploy
```
Verify Operator Created
```
kubectl get pods -n awx
```

Deploy AWX
```

vi my-awx.yml
---
apiVersion: awx.ansible.com/v1beta1
kind: AWX
metadata:
  name: awx-demo
spec:
  service_type: nodeport
```
```
kubectl apply -f my-awx.yml -n ansible-awx
```
Verify 
```
kubectl get awx -n ansible-awx

```
vi my-awx-ingress.yml
```
---
apiVersion: awx.ansible.com/v1beta1
kind: AWX
metadata:
  name: awx-demo
spec:
  service_type: nodeport
  ingress_type: ingress
  hostname: awx.test.me
```
```
kubectl apply -f my-awx-ingress.yml
```

