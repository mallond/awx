# Microk8s Install

> So simple, that your grandma can do it. Git it a try.

## A few things
```
sudo systemctl status ufw.service
sudo systemctl status sshd.service
```

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

## AWX


