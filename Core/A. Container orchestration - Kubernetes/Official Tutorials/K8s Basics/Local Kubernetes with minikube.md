# Setup for Beginners


[minikube](https://minikube.sigs.k8s.io/docs/start/?arch=/macos/arm64/stable/binary+download) is local Kubernetes, focusing on making it easy to learn and develop for Kubernetes.

Use case: testing apps / features in local environment with scarce resources (CPU, memory, time for deploying complex cluster)


minikube is a single-node K8s cluster where Master and Worker processess runs on this one node with pre-installed Docker.


* minikube CLI is used for start up/deleting the cluster.
* kubectl CLI is used for configuring the minikube cluster.



[kubectl]() -- tool for interaction with K8s cluster. One of the 3 clients for API Server (UI, API, CLI=kubectl), the most powerful one.


kubectl -> API Server -> Worker Processes -> [pods/services operations]




