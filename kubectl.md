
## Kubectl commands
`k get nodes`
```
NAME       STATUS   ROLES                  AGE     VERSION
minikube   Ready    control-plane,master   2m18s   v1.20.2
```
`k version`
```
Client Version: version.Info{Major:"1", Minor:"21", GitVersion:"v1.21.0", GitCommit:"...", GitTreeState:"clean", BuildDate:"2021-04-08T21:15:16Z", GoVersion:"go1.16.3", Compiler:"gc", Platform:"darwin/amd64"}
Server Version: version.Info{Major:"1", Minor:"20", GitVersion:"v1.20.2", GitCommit:"...", GitTreeState:"clean", BuildDate:"2021-01-13T13:20:00Z", GoVersion:"go1.15.5", Compiler:"gc", Platform:"linux/amd64"}
```
## Create a Nginx deployment
Pod is the smalles unit, but you do not work directly with it but with a Deployment.
`k create deployment nginx-depl --image=ngix`
loads and download from the DockerHub the image for nginx
this is a blueprint for creating pods
most basic configuration for deployment (name and images to use)
the rest is managed by k8s
```
deployment.apps/nginx-depl created
```
`k get replicaset`
replicaset manages the repicas of a Pod
here we created 1 replica but this can be adjusted in the yaml file
Deployment manages a ...
Replicaset manages a ...
Pod is an abstraction of ...
Docker container
```
NAME                    DESIRED   CURRENT   READY   AGE
nginx-depl-7fc99fc5d4   1         1         1       35s
```
`k get pods -A --watch`
```
NAMESPACE     NAME                               READY   STATUS    RESTARTS   AGE
default       nginx-depl-7fc99fc5d4-9nzkv        1/1     Running   0          17h
default       nginx-deployment-f4b7sdcbc-ch2fq   1/1     Running   0          13h
```
`k edit deployment nginx-depl`
```
// we add the version to the nginx deployment
      containers:
      - image: nginx:1.16
        imagePullPolicy: Always
...
deployment.apps/nginx-depl edited
```
`k apply -f \ nginx-deployment.yaml `
makes deployment from file
```
deployment.apps/nginx-deployment created
```
## Create a MongoDB deployment and logging
`k create deployment mongo-depl --image=mongo`
```
NAME                          READY   STATUS              RESTARTS   AGE
mongo-depl-5fd6b7d4b4-k5w6w   0/1     ContainerCreating   0          23s
nginx-depl-7fc44fc5d4-mf4kk   1/1     Running             0          4m13s
```
### Use describe to check deployment status
`k describe mongo-depl-5fd6b7d4b4-k5w6w`
```
Name:         mongo-depl-5fd6b7d4b4-k5w6w
Namespace:    default
Priority:     0
Node:         minikube/192.xxx.xx.x
Start Time:   Wed, 02 Jun 2021 13:07:18 +0200
Labels:       app=mongo-depl
              pod-template-hash=5fd6b7d4b4
Annotations:  <none>
Status:       Running <====
IP:           172.xx.x.x
```
### Display log output
`k logs mongo-depl-5fd6b7d4b4-k5w6w`
```
{"t":{"$date":"2021-06-02T11:10:54.350+00:00"},"s":"I",  "c":"STORAGE",  "id":22430,   "ctx":"WTCheckpointThread","msg":"WiredTiger message","attr":{"message":"[asdfasdf:35asdf0181][1:asdfasdfasdfsd], WT_SESSION.checkpoint: [WT_VERB_CHECKPOINT_PROGRESS] saving checkpoint snapshot min: 37, snapshot max: 37 snapshot count: 0, oldest timestamp: (0, 0) , meta checkpoint timestamp: (0, 0)"}}
```
### Interactive terminal in container
`k exec -it mongo-depl-5fd6b7d4b4-k5w6w -- bin/bash`
```
root@mongo-depl-5fd6b7d4b4-k5w6w:/# ls
bin   data  docker-entrypoint-initdb.d  home        lib    media  opt   root  sbin  sys  usr
boot  dev   etc                         js-yaml.js  lib64  mnt    proc  run   
```
### Delete deployment
`k delete deployment mongo-depl-5fd6b7d4b4-k5w6w`