# Setup for Beginners


[minikube](https://minikube.sigs.k8s.io/docs/start/?arch=/macos/arm64/stable/binary+download) is local Kubernetes, focusing on making it easy to learn and develop for Kubernetes.

Use case: testing apps / features in local environment with scarce resources (CPU, memory, time for deploying complex cluster)


minikube is a single-node K8s cluster where Master and Worker processess runs on this one node with pre-installed Docker.

* minikube CLI is used for start up/deleting the cluster.

```sh
minikube start
minikube status
minikube stop
```



[kubectl]() -- tool for interaction with K8s cluster. One of the 3 clients for API Server (UI, API, CLI=kubectl), the most powerful one.


kubectl -> API Server -> Worker Processes -> [pods/services operations]

* kubectl CLI is used for configuring the minikube cluster.

```sh
kubectl get nodes
kubectl version
```



# Basic kubectl commands

```sh
# List all components in the cluster:
kubectl get nodes
kubectl get pods
kubectl get pod -o wide # more info
kubectl get services
...
```

#### Creation
```sh
# List all components that kubectl can create:
kubectl create -h
```


```sh
# Create a pod (deployment is an abstraction for pods, we don't create pods directly):
kubectl create deployment NAME --image=IMAGE [--dry-run] [options]
```
* `--dry-run` flag is used to test the command without actually creating the pod.



Pod name is generated in this way:
[deployment-name]-[replica-set-id]-[pod-id]
* example: ```kubectl create deployment nginx-d --image=nginx```
* [1]deployment: ```nginx-d```
* [2]replicaset: ```nginx-d-7d547dd87b```
* [3]pod: ```nginx-d-7d547dd87b-s9xjm```
* [4]lowest level of abstraction: container

Replicaset manages the number of pods that should be running at any given time.

#### Editing deployment file (Read, Update)
```sh
# using default editor
kubectl edit deployment NAME 
# with a specific editor
KUBE_EDITOR="nano" kubectl edit deployment NAME
# with a specific format, e.g. json (yaml is default)
kubectl edit deployment NAME -o=json
```

#### Debugging pods
```sh
kubectl get pod --watch
kubectl logs POD_NAME
kubectl describe pod POD_NAME
kubectl exec -it POD_NAME -- bin/bash

```

#### Deleting resources (through abstraction level of deployment)
```sh
kubectl delete deployment NAME
```

#### Advanced creation/deletion
```sh
# Create a pod from a config file:
kubectl apply -f config.yaml
# Delete with a config file:
kubectl delete -f config.yaml
```