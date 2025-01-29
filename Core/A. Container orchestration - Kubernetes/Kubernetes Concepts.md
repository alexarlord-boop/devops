# Kubernetes Concepts
Helmsman/pilot = K[ubernete]s = K8s
* [Kubernetes Essentials (by IBM Technology)](https://youtube.com/playlist?list=PLOspHqNVtKABAVX4azqPIu6UfsPzSu2YN&si=LjQj6ssdtmJ4h93A)
* [K8s Documentation](https://kubernetes.io/docs/home/)
  * [Concepts Overview](https://kubernetes.io/docs/concepts/)




## What for?
* Bundle and run apps.
* Mitigate downtime of complex containerized applications in production.
* Remove the need for manual intervention in deploying, scaling, and managing these apps.
* Kubernetes helps automate distributed system behavior in a resilient, scalable, manageable, declarative way.

## How?
By providing an extensible platform that allows you to:
1. Balance load
2. Scale horizontally
3. Orchestrate storage
4. Automate path to desired state
5. Self-heal
6. Manage secrets, configs, resources, IPs, etc.

## Limitations
* Not a PaaS (Platform as a Service) solution.
* CI/CD, monitoring, logging, etc. are not built-in.
* No built-in services like databases, message queues, etc.

## [Components](https://kubernetes.io/docs/concepts/overview/components/)
* ```Pod``` = 1+ containers with shared storage and network resources.
* ```Cluster``` = control plane AND 1+ worker nodes with running pods.
#### Control Plane -- its components manages overall state of a cluster
1. ```kube-apiserver``` = exposes K8s API.
2. ```etcd``` = distributed key-value store for all cluster data - "the cluster brain".
3. ```kube-scheduler``` = schedules pods, assigns to Node.
4. ```kube-controller-manager``` = runs ```controllers``` (watch-loop that tracks current state and attempts to achieve desired one).
5. ```cloud-controller-manager``` = interacts with underlying cloud providers (optional).

#### Worker Node -- its components maintain pods in a K8s running environment.
1. ```kubelet``` = agent that runs on each node, ensures pods + its containers are running.
2. ```kube-proxy``` = maintains network rules on nodes to make them operate as a service. (optional)
3. ```Container Runtime``` = software to run containers (containerd, CRI-O, etc.) ([what are container runtimes?](https://www.wiz.io/academy/container-runtimes))

* ```minikube``` = a tool that allows you to set up a single-node Kubernetes cluster on your local computer. [Install](https://minikube.sigs.k8s.io/docs/start/?arch=/macos/arm64/stable/binary+download)
* ```kubectl``` = CLI to interact with K8s cluster. [Install](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

#### [Add-ons](https://kubernetes.io/docs/concepts/architecture/#dns): DNS, Web UI, Container Resource Monitoring, Cluster-level Logging


## Cluster Architecture
* >Cluster
  >>Control Plane
    >>* API Server
    >>* Scheduler
    >>* etcd
    >>* Controller Manager
    >>* cloud-controller-manager
  >
  >>Worker Node N
    >>* Kubelet
    >>* Kube Proxy
    >>* Container Runtime
    >>  * Pods
    >>    * Containers


## Namespaces
* helps organize objects in the cluster.

```bash
kubectl get namespaces
```

Main namespaces, created by default:
* ```default``` = if no namespace is specified.
* ```kube-system``` = K8s system objects, processess.
* ```kube-public``` = public info about the cluster.
* ```kube-node-lease``` = heartbeats of nodes -- availability.

#### Creation
```bash
kubectl create namespace my-namespace
```
a better option for documented creation - use in yaml K8s configuration:
```yaml
...
metadata:
  name: my-namespace
...
```


#### Use cases

1. Organizing components, resources for better overview and management: 
  * databases, 
  * monitoring, 
  * elastic stack, logging, etc.

2. Avoiding conflicts with other teams: 
  * same deployment name, but different configs 
  
3. Sharing resources
  * Staging, development, production environments that uses same ELK
  * Blue/Green deployments: different app versions, but usage of same resources.

4. Limit access to resources: 
  * team A can't see resources of team B.
  * CPU, RAM, Storage quotas per namespace


#### Limitations
1. ConfigMaps, Secrets for shared resources are not shareable, we need to create them in each namespace.
2. Only services are accessible across namespaces.
3. You can't namespace volumes, nodes.

```bash
kubectl api-resources --namespaced=false
kubectl api-resources --namespaced=true
```


#### Usage
To address resources in a specific namespace, use ```-n``` or ```--namespace``` flag:
```bash
kubectl get pods -n my-namespace
```

Install tool ```kubens``` (kubectx for macOS) to switch between namespaces easily and skip namespace name mention.

```bash
kubens # list namespaces
kubens my-namespace # switch to namespace
```
