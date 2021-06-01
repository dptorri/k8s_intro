# k8s intro

Some introductory commands in minikube and examples.
Note: `kubectl`  uses the alias `k` for simplicity

## How to setup Minikube (OSX) :package:

- `minikube start --hyperv-virtual-switch=minikube`

- `minikube status`

- `minikube profile list`

- `minikube stop`

- `minikube delete`

## Kubectl commands

- `k get nodes`

- `k version`

## Create a Nginx deployment

- `k create deployment nginx-depl --image-ngnix`

- `k get pod`

- `k get replicaset`


