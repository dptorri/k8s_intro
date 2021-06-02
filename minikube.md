
## How to setup Minikube (OSX) :package:
`minikube start --driver=docker`
```
😄  minikube v1.19.0 on Darwin 10.15.7
✨  Automatically selected the docker driver. Other choices: hyperkit, ssh
👍  Starting control plane node minikube in cluster minikube
🔥  Creating docker container (CPUs=2, Memory=1987MB) ...
🐳  Preparing Kubernetes v1.20.2 on Docker 20.10.5 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```
`minikube status`
```
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```
`minikube profile list`
```
|----------|-----------|---------|--------------|------|---------|---------|-------|
| Profile  | VM Driver | Runtime |      IP      | Port | Version | Status  | Nodes |
|----------|-----------|---------|--------------|------|---------|---------|-------|
| minikube | docker    | docker  | 192.xxx.xx.x | 8443 | v1.20.2 | Running |     1 |
|----------|-----------|---------|--------------|------|---------|---------|-------|
```
`minikube stop`
```
✋  Stopping node "minikube"  ...
🛑  Powering off "minikube" via SSH ...
🛑  1 nodes stopped.
```
`minikube start`
```
😄  minikube v1.19.0 on Darwin 10.15.7
✨  Using the docker driver based on existing profile
👍  Starting control plane node minikube in cluster minikube
🔄  Restarting existing docker container for "minikube" ...
🐳  Preparing Kubernetes v1.20.2 on Docker 20.10.5 ...| 
```
`minikube delete`
```
🔥  Deleting "minikube" in docker ...
🔥  Deleting container "minikube" ...
🔥  Removing /Users/torrid/.minikube/machines/minikube ...
💀  Removed all traces of the "minikube" cluster.
```