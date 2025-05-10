# Kubernetes Kinds

---

## **1. Workload Management (Running Applications)**  
- **Pod** – The smallest deployable unit, containing one or more containers.  
- **Deployment** – Manages stateless applications, supports rolling updates and rollbacks.  
- **ReplicaSet** – Ensures a specified number of Pod replicas are running.  
- **StatefulSet** – Manages stateful applications, ensuring stable identities and persistent storage.  
- **DaemonSet** – Ensures a Pod runs on every (or specific) node, often used for system services.  
- **Job** – Runs batch processes to completion.  
- **CronJob** – Schedules Jobs to run at specific times.  

---

## **2. Networking & Traffic Control**  
- **Service** – Provides stable network access to a set of Pods.  
- **Ingress** – Manages external HTTP/HTTPS access with routing rules.  
- **NetworkPolicy** – Defines rules for network communication between Pods.  
- **Endpoint & Endpoints** – Defines the actual Pod IPs backing a Service.  

---

## **3. Scaling & Availability**  
- **HorizontalPodAutoscaler (HPA)** – Scales Pods based on CPU/memory usage or custom metrics.  
- **VerticalPodAutoscaler (VPA)** – Adjusts CPU/memory requests and limits for Pods dynamically.  
- **PodDisruptionBudget** – Defines how many Pods can be disrupted during maintenance.  
- **PriorityClass** – Assigns priority levels to Pods for scheduling and eviction decisions.  

---

## **4. Security & Access Control**  
- **Namespace** – Provides logical isolation within a cluster.  
- **Role & ClusterRole** – Define RBAC permissions at the namespace or cluster level.  
- **RoleBinding & ClusterRoleBinding** – Assign roles to users or service accounts.  
- **ServiceAccount** – Grants authentication to Pods interacting with the Kubernetes API.  

---

## **5. Node & Cluster Administration**  
- **Node** – A worker machine in the cluster where Pods run.  
- **ComponentStatus** – Displays the health of core cluster components.  
- **Event** – Logs occurrences and state changes in the cluster.  

---

