# K8s Configuration (YAML file)
Kubernetes (K8s) YAML configuration files define the desired state of your application in terms of resources, such as Deployments, Services, ConfigMaps, and more. 
Each Configuration file has 5 parts:

1. **API Version**: The API version of the Kubernetes API you're using to create the object.
2. **Kind**: The kind of object you want to create (deployment, service, etc.).
3. **Metadata**: Data that helps uniquely identify the object, including a name string, UID, and optional namespace.
4. **Spec**: The desired state of the object, which is the configuration you want to apply to the object.
5. **Status**: The current state of the object, supplied and updated by the Kubernetes API (automatically generated).

Deployment manages a ReplicaSet, which in turn manages Pods. The Deployment ensures that the desired number of Pods are running and available at all times -- template section within the spec defines the Pod blueprint that the Deployment will use to create new Pods. It also has its own metadata and spec sections.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
    replicas: 3
    selector:
        matchLabels:
        app: my-app
    template:
        metadata:
        labels:
            app: my-app
        spec:
        containers:
        - name: my-app
            image: my-app:latest
            ports:
            - containerPort: 80
```

## Kinds of objects
1. **Pod**: A group of one or more containers, with shared storage/network resources, and a specification for how to run the containers.
2. **Service**: An abstraction that defines a logical set of Pods and a policy by which to access them.
3. **Volume**: A directory that contains data accessible to containers in a Pod.
4. **Namespace**: A way to divide cluster resources between multiple users.
5. **ConfigMap**: A way to inject configuration data into Pods.
6. **Secret**: A way to inject sensitive data into Pods.
7. **PersistentVolume**: A piece of storage in the cluster that has been provisioned by an administrator.

## Deployment and Service breakdown
* A Deployment is a Kubernetes resource that defines how to manage a group of Pods (running instances of your application). It ensures that the correct number of Pods are running, replaces failed Pods, and updates Pods in a controlled way (e.g., rolling updates).
* A Service is a Kubernetes resource that provides a stable network endpoint (a consistent IP address or DNS name) to access one or more Pods.
Pods are ephemeral (they can be created or destroyed at any time), but a Service ensures the application is always accessible.

#### How they work together?
1. Deployment:
> * Ensures your application (Pods) is running as desired.
> * Handles scaling, updates, and restarting failed Pods.

2. Service:
> * Exposes the Pods created by the Deployment so they can communicate with other components (or the outside world).
> * Provides load balancing to distribute traffic across the Pods.

#### Example
* Deployment creates 3 Pods running your application.
* Service matches the Pods using a label (e.g., app: my-app) and provides a stable endpoint for traffic, e.g external load balancer on AWS.

#### Analogy
* Deployment: Think of it as a factory that produces and manages identical cars (Pods).
* Service: Acts like the dealership or delivery service that connects customers to the cars.


## Connecting components: Labels, Selectors, Ports
* Metadata section contains ```labels```
* Specification section contains ```selectors```


1. Labels are key-value pairs that are attached to objects, such as Pods, through the template blueprint.
2. Selectors are used to filter objects based on labels. For example, the Deployment object uses a selector to find all Pods with the label app=my-app.




```yaml
# Deployment file
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
    replicas: 3
    selector:
        matchLabels:
        app: my-app # used by service selector
    template:
        metadata:
        labels:
            app: my-app
        spec:
        containers:
        - name: my-app
          image: my-app:latest
          ports:
          - containerPort: 8080 # container listens on port 8080
```

```yaml
# Service file
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
    selector:
        app: my-app # references the label of deployment
    ports:
    - protocol: TCP
        port: 80
        targetPort: 8080 # references the containerPort of deployment

```

Ports are used to expose services to the outside world. The containerPort field specifies the port that the container listens on. In this example, the container listens on port 8080.


