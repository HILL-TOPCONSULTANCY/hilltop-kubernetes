  **Author: Prince Chafah Forchu Sani**
  
  [Profile](https://www.linkedin.com/in/prince-chafah-forchu-sani-691534158/)
  # KUBERNETES:

## **INTRODUCTION**

+ Kubernetes (often abbreviated as K8s) is an open-source platform designed to automate the deployment, scaling, and operation of containerized applications.
+ It provides a robust framework to run distributed systems resiliently, managing the complexity of microservice architectures.
+ ensuring that containerized applications run smoothly in various environments, from local development to large-scale production.

## History

+ Kubernetes was originally developed by Google and is based on their experience running containerized applications in production.
+ Google introduced Kubernetes as an open-source project in 2014. It is written in Go and has since become one of the most popular container orchestration platforms.
+ The project is now maintained by the Cloud Native Computing Foundation (CNCF), which has fostered its growth and adoption across the industry.

## **Key Features of Kubernetes**
+ Kubernetes offers a robust set of features designed to meet the needs of modern containerized applications:

### 1. **Scaling**

+ Kubernetes dynamically adjusts the number of running containers based on demand, ensuring optimal resource utilization.
+ This adaptability helps reduce costs while maintaining a smooth user experience.

### 2. **Load Balancing**

+ Load balancing is integral to Kubernetes. It effectively distributes incoming traffic across multiple pods, ensuring high availability and optimal performance and preventing any single pod from becoming overloaded.

### 3. **Self-Healing**

+ Kubernetes’ self-healing capabilities minimize downtime. If a container or pod fails, it is automatically replaced, keeping your application running smoothly and ensuring consistent service delivery.

### 4. **Service Discovery and Metadata**

+ Service discovery is streamlined in Kubernetes, facilitating communication between different application components. Metadata enhances these interactions, simplifying the complexities associated with distributed systems.

### 5. **Rolling Updates and Rollbacks**

+ Kubernetes supports rolling updates to maintain continuous service availability. If an update causes issues, reverting to a previous stable version is quick and effortless.

### 6. **Resource Management**

+ Kubernetes allows for precise resource management by letting you define resource limits and requests for pods, ensuring efficient use of CPU and memory.

### 7. **ConfigMaps, Secrets, and Environment Variables**

+ Kubernetes uses ConfigMaps and Secrets for secure configuration management. These tools help store and manage sensitive information such as API keys and passwords securely, protecting them from unauthorized access.


**KUBERNETES INSTALLATION:**
1. [Install Kubelet on Windows](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/)
2. KOPS
3. [Install Kubeadm](https://github.com/HILL-TOPCONSULTANCY/kubernetes/blob/main/README.md) (self managed)
4. EKS (managed)
5. AKS
6. GKE
7. Creat 3 Ubuntu EC2 Instances of t3 Medium

## KUBERNETES ARCHITECTURE:

### ControlPlane: MasterNode
     
1. **ApiServer:**
    + Primary mgnt component/ exposes k8s API serving as frontend interface for all cluster operations, handles restful Api from kubelet
    + When you run a kubectl command, it communicates with the kube API and then it gets the data from the ETCD
    + The request is first authenticated and then validated.
2. **ETCD:**
    + It is a key-value store, It stores the cluster's configuration data.
3. **Scheduler:**
    + Responsible for making decisions about pod placement on worker nodes in the cluster.
    + It examines resource requirements, quality-of-service constraints, affinity, anti-affinity, and other policies to determine the most suitable node for running a pod.
    + it doesn't place resources on nodes but makes the decision
4. **ControllerManagers:**
   + Manages Node lifecycle, desired pod number, and services it continuously monitors the state of resources in the cluster and ensures that they match the desired state.
   + Node Controler: monitors the status of the nodes every 5sec. it waits for 40 secs and if unreachable, it evicts the pods running on the node.
5. **ReplicationController:**
   + it monitors the status of the replica set and makes sure the desired states are maintained.

         ReplicaSet
         DaemonSet
         ReplicationController
     
 ## Node Components:
 
1. **kubelet:** 
+ The Kubelet is responsible for managing and ensuring the running of containers created by Kubernetes. communicate with control-plane
+ it registers nodes into the cluster. It monitors nodes and pods in the cluster every 10 minutes and  relates feedback to the API which is stored in the etcd.cluster
2. **container runtime:**
+ A fundamental component that empowers Kubernetes to run containers effectively. It is responsible for managing the execution and lifecycle of containers within the Kubernetes environment.
   [Container-d] docker pulling containered images
3. **kube-proxy:**
+ enables network communication and load balancing between pods and services within the cluster.
+ every pod in a cluster can communicate with other pods in the cluster by using IP address of the pod.
+ to access each pod, a service has to be created and then you can access the pod by using the service name.
+ the service is not an object, the kube-proxy creates rules that allow traffic routing within the cluster.

         ClusterIP
         NodePort
         LoadBalancer
+ kubernetes-client:
```bash
  kubectl  
      kubectl create/delete/get/describe/apply/run/expose 
``` 
      kubeconfig [.kube/config ] file will authenticate 
                                 the caller admin/Developer/Engineer 

In Kubernetes Quality of Service (QoS) refers to the priority and resource guarantees provided to different 
workloads or pods within a cluster. Kubernetes provides mechanisms to manage QoS to ensure that critical 
workloads receive the necessary resources and performance, while also allowing for efficient resource 
utilization.

# POD AND CONTROLLERS - KUBERNETES WORKLOAD:

### 1. *PODS:*
A **Pod** is the smallest and simplest unit of deployment in Kubernetes. It represents a single instance of a running application in your cluster and is the basic building block for workloads in Kubernetes. A Pod encapsulates one or more tightly coupled containers, their shared storage, network, and configuration.

---

### **Key Features of a Pod**
1. **Single or Multiple Containers**:
   - A Pod typically runs a **single container**, but it can also include **multiple containers** that need to share resources (e.g., sidecar patterns).
   - All containers in a Pod share the same network namespace and storage.

2. **Shared Resources**:
   - **Network**:
     - Containers in a Pod share the same IP address and port space, meaning they communicate with each other over `localhost`.
   - **Storage**:
     - Pods can share **volumes** for persistent or temporary storage between containers.

3. **Ephemeral by Nature**:
   - Pods are ephemeral and not designed to be durable. If a Pod dies, it is not automatically recreated unless managed by a higher-level object like a **Deployment** or **ReplicaSet**.

4. **Lifespan**:
   - Pods have a finite lifespan. They are created, run, and terminated when no longer needed or when the node they’re on fails.

---

### **Pod Structure**

Here’s an example of a Pod definition in YAML:
Here's a complete and **clearly explained Kubernetes YAML example** that:

1. Deploys an `nginx` Pod using a **Deployment**
2. Exposes it using a **Service**
---
### `nginx-pod.yaml`
```yaml
apiVersion: v1                    # This is the API version for basic Kubernetes resources like Pods
kind: Pod                         # The object type we're creating is a Pod
metadata:
  name: nginx-pod                 # Name of the Pod
  labels:
    app: nginx                    # Labels help identify and group this Pod
spec:
  containers:                     # List of containers in the Pod (even one container is in a list)
    - name: nginx                 # Name of this container
      image: nginx               # Docker image to use (nginx is publicly available on Docker Hub)
      ports:
        - containerPort: 80      # The port the container exposes
```

### `nginx-deployment.yaml`

```yaml
apiVersion: apps/v1                # Specifies the version of the Kubernetes API to use for Deployment
kind: Deployment                   # The type of object you're creating (Deployment manages Pods)
metadata:
  name: nginx-deployment           # Name of the Deployment
  labels:
    app: nginx                     # Labels help identify and group Kubernetes objects
spec:
  replicas: 2                      # How many nginx Pods you want
  selector:
    matchLabels:
      app: nginx                   # Tells the Deployment to manage Pods with this label
  template:                        # This defines the Pod template
    metadata:
      labels:
        app: nginx                 # Labels applied to the Pods created
    spec:
      containers:                  # List of containers in the Pod (here, just one)
        - name: nginx              # Name of the container
          image: nginx             # Docker image to use
          ports:
            - containerPort: 80    # Port the container listens on
```

---

### `nginx-service.yaml`

```yaml
apiVersion: v1                     # This time we use 'v1' for core resources like Service
kind: Service                      # We're creating a Service to expose our app
metadata:
  name: nginx-service              # Name of the Service
spec:
  selector:
    app: nginx                     # This tells the Service to target Pods with this label
  ports:
    - protocol: TCP                # Protocol used
      port: 80                     # Port on the Service
      targetPort: 80               # Port on the Pod the Service should forward to
  type: ClusterIP                  # Type of Service (ClusterIP = internal-only access)
```

---

##  Explanation of Key Components

| **Component**     | **Explanation**                                                                                                                                        |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `apiVersion`      | The API group and version to use. For Deployments, it's `apps/v1`. For Services, it's `v1`. This ensures Kubernetes knows what features are supported. |
| `kind`            | The type of Kubernetes object you're creating: `Deployment` or `Service`.                                                                              |
| `metadata`        | Contains the name and optional labels for identifying the object.                                                                                      |
| `spec`            | The desired state of the object. Each `kind` has a specific format for this.                                                                           |
| `replicas`        | Number of Pod copies you want.                                                                                                                         |
| `selector`        | Used to match Pods with specific labels. Helps controllers like `Deployment` or `Service` know which Pods to manage or expose.                         |
| `template`        | Only in `Deployment`. It's a blueprint for creating Pods.                                                                                              |
| `containers`      | A **list** (notice the `-`) of container definitions. Even if only one container is used, the YAML uses a list format.                                 |
| `image`           | The container image to deploy (e.g., `nginx`).                                                                                                         |
| `containerPort`   | Port that the app inside the container listens to.                                                                                                     |
| `ports` (Service) | List of port rules for how to forward traffic.                                                                                                         |
| `type` (Service)  | Defines how the Service is exposed: `ClusterIP`, `NodePort`, `LoadBalancer`, etc.                                                                      |
---

### **Pod Lifecycle**
1. **Pending**: The Pod is accepted by the cluster but is waiting for resources to be allocated.
2. **Running**: The Pod is bound to a node, and all containers are running.
3. **Succeeded**: All containers in the Pod have completed successfully.
4. **Failed**: All containers in the Pod have terminated with a failure.
5. **Unknown**: The state of the Pod cannot be determined.

---

### **Why Use Pods?**
- Pods group **tightly coupled containers** that need to:
  - Share storage volumes.
  - Share a network namespace.
  - Work together as part of the same application component.
  
- **Example Use Cases**:
  - A main application container with a **sidecar container** for logging.
  - Running a web server container alongside a caching container.

---

### **Pods and Higher-Level Abstractions**

Pods are often managed by higher-level Kubernetes resources:

1. **Deployments**:
   - Ensures Pods are replicated, updated, and self-healing.
2. **ReplicaSets**:
   - Manages a fixed number of Pod replicas.
3. **Jobs** and **CronJobs**:
   - For running Pods that perform specific tasks and exit (batch processing).

---

### **Pods vs Containers**

| **Feature**         | **Pod**                                      | **Container**                               |
|----------------------|----------------------------------------------|---------------------------------------------|
| **Scope**           | A wrapper that can manage multiple containers. | A single lightweight runtime process.       |
| **IP Address**      | Assigned one IP address for all containers inside. | Each container has its own IP (outside Kubernetes). |
| **Persistence**     | Can share persistent storage between containers. | Containers are isolated by default.         |

---

### **How to Create a Pod**

1. **Using YAML**:
   Save the example YAML file as `pod.yaml`:
   ```bash
   kubectl apply -f pod.yaml
   ```

2. **Using `kubectl` Command**:
   ```bash
   kubectl run my-app --image=nginx:1.23.1 --port=80
   ```

---

### **Common Commands for Pods**
1. **List Pods**:
   ```bash
   kubectl get pods
   ```

2. **Describe a Pod**:
   ```bash
   kubectl describe pod <pod-name>
   ```

3. **Delete a Pod**:
   ```bash
   kubectl delete pod <pod-name>
   ```

4. **Access a Pod**:
   ```bash
   kubectl exec -it <pod-name> -- /bin/bash
   ```

5. **View Pod Logs**:
   ```bash
   kubectl logs <pod-name>
   ```
---

# Apply the configuration and access on the browser
```sh
kubectl get pods -o wide
kubectl get service -o wide
```

+ Copy the public IPV4 Address of the pod node hosting the pod
+ Access the UI of the application using curl _**http://(public-ip):(NodePort)**_
```sh
- kubectl apply/create -f <filename> #to create declaratively from a yml file
- kubectl get/describe pods <podname> #to get the pod spec
- kubectl create deployment redis-deployment --image=redis123 --dry-run=client -o yaml > deployment.yaml
- kubectl create deployment redis-deployment --image=redis123 -o yaml #print out output
- kubectl edit pod <podname>
```
### 2. *DEPLOYMENTS:*

A **Deployment** in Kubernetes is a resource object that provides declarative updates for applications. 
It is used to manage and maintain the desired state of an application running in a Kubernetes cluster. Deployments handle the lifecycle of **Pods** and ensure your application runs reliably by automatically replacing failed or unhealthy pods and scaling the application when needed.

---

### **Key Features of a Deployment**
1. **Declarative Updates**:
   - You specify the desired state of your application (e.g., the number of replicas, container image version), and Kubernetes ensures that this state is achieved and maintained.

2. **Rolling Updates**:
   - Deployments update your application incrementally (rolling out new versions while keeping the application available).

3. **Self-Healing**:
   - If a pod crashes or becomes unhealthy, the Deployment ensures new pods are created to maintain the desired number of replicas.

4. **Scaling**:
   - You can scale your application up or down by increasing or decreasing the number of replicas.

5. **Versioning and Rollbacks**:
   - Deployments keep track of revision history, allowing you to roll back to previous versions of your application.

---

### **Structure of a Deployment**

A Deployment is defined in a YAML or JSON file. Below is an example YAML file:


### `deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: devops-app-deployment
  labels:
    app: devops-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: devops-app
  template:
    metadata:
      labels:
        app: devops-app
    spec:
      containers:
      - name: devops-app-container
        image: hilltopconsultancy/docker:v2
        ports:
        - containerPort: 8080
```

#### Explanation:
- **`apiVersion`**: Specifies the API version (`apps/v1` is standard for Deployments).
- **`kind`**: Defines the resource type (`Deployment`).
- **`metadata`**: Contains the name and labels for the Deployment.
- **`spec`**:
  - **`replicas`**: Number of pod replicas to maintain.
  - **`selector`**: Identifies the pods managed by this Deployment (based on matching labels).
  - **`template`**: Defines the pod's specification (labels, containers, image, ports, etc.).

---

### **Key Operations with Deployments**

#### **1. Create a Deployment**
Apply the Deployment YAML file to the cluster:
```bash
kubectl apply -f deployment.yaml
```

#### **2. Check Deployment Status**
View details about the Deployment:
```bash
kubectl get deployments
```

#### **3. Scale a Deployment**
Change the number of replicas in the Deployment:
```bash
kubectl scale deployment devops-app-deployment --replicas=5
```

#### **4. Update a Deployment**
Modify the Deployment (e.g., change the container image in the YAML file) and apply it again:
```bash
kubectl apply -f deployment.yaml
```

#### **5. Roll Back a Deployment**
Revert to the previous version of the Deployment:
```bash
kubectl rollout undo deployment devops-app-deployment
```

#### **6. Delete a Deployment**
Remove the Deployment and its associated pods:
```bash
kubectl delete deployment devops-app-deployment
```

---

### **Use Cases for Deployments**
1. **Stateless Applications**:
   - Deployments are commonly used for stateless applications like web servers or APIs.
2. **Zero-Downtime Updates**:
   - Rolling updates ensure no downtime during application updates.
3. **High Availability**:
   - By managing replicas, Deployments ensure your application is always available.
4. **Testing New Versions**:
   - Deployments make it easy to test new versions of your app and roll back if issues arise.

---

### **Deployment vs Pod**
| **Feature**         | **Deployment**                                         | **Pod**                                   |
|----------------------|-------------------------------------------------------|-------------------------------------------|
| **Purpose**          | Manages pods and ensures the desired state is maintained. | Represents a single instance of your application. |
| **Self-Healing**     | Automatically recreates pods if they fail.            | A pod does not automatically restart if it crashes. |
| **Scaling**          | Easy to scale up or down by adjusting replicas.       | Scaling must be managed manually.         |
| **Rolling Updates**  | Supports rolling updates for zero downtime.           | No direct update mechanism.               |

---


### `deploy.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hilltop-app-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: hilltop-app
  template:
    metadata:
      labels:
        app: hilltop-app
    spec:
      containers:
      - name: hilltop-container
        image: hilltopconsultancy/docker:v2
        ports:
        - containerPort: 8080
```

### `deploy-service.yaml`
```yaml
apiVersion: v1
kind: Service
metadata:
  name: hilltop-svc
spec:
  selector:
    app: hilltop-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 30035
  type: NodePort
EOF
```
# Deployment Commands
```sh
kubectl get deployment
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml
kubectl set image deployment/nginx-deployment nginx=nginx:1.16.1
kubectl edit deployment/nginx-deployment
kubectl rollout status deployment/nginx-deployment
kubectl rollout undo deployment/nginx-deployment --to-revision=2
kubectl get rs
kubectl get pods
kubectl describe deployments
kubectl scale deployment/nginx-deployment --replicas=10
```
# 3. *REPLICASETS:*
A ReplicaSet maintains a stable set of replica Pods running at any given time. As such, it is often used to guarantee the availability of a specified number of identical Pods
Controllers are the brain behind k8s, they monitor k8s objects and respond accordingly.
- the replication controller helps increase the number of pods in the node for high availability.
- It also serves as recreating a pod if it fails.
- It creates pods across nodes to balance the load
- replication controller is replaced by replicasets
- it maintains the desired number of pods specified in your object definition file 

### `rs.yaml`
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
spec:
  replicas: 3 
  selector:
    matchLabels:
      app: myapp  
  template:
    metadata:
      labels:
        app: myapp 
    spec:
      containers:
        - name: myapp-container
          image: hilltopconsultancy/docker:v2
          ports:
            - containerPort: 8080       
```
```sh
- kubectl create -f <filename>
- kubectl get rc 
```
+ replicasets requires a selector field | it is not a must
+ It helps the replica set define what pods fall under it although pod spec has already been mentioned in the spec
+ This is because it can manage pods that were not created to be managed by the rs

`svc.yaml`
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  selector:
    app: myapp  # Matches the Pods managed by the ReplicaSet
  ports:
    - protocol: TCP
      port: 80        # External service port
      targetPort: 80  # Port inside the container
  type: LoadBalancer  # Use 'NodePort' for internal access
```
### AUTOSCALER
https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/
You can use a Horizontal Pod AutoScaler (HPA)

+ kubectl autoscale rs myapp-rc --cpu-percent=50 --min=1 --max=10
### `autoscalaer.yaml`
```yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: frontend-scaler
spec:
  scaleTargetRef:
    kind: ReplicaSet
    name: myapp-rc
  minReplicas: 3
  maxReplicas: 10
  targetCPUUtilizationPercentage: 50
```

You can generate load on your pods by running the command

+ Run this in a separate terminal
+ So that the load generation continues and you can carry on with the rest of the steps
```sh
kubectl run -i --tty load-generator --rm --image=nginx --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://php-apache; done"
```
```sh
# type Ctrl+C to end the watch when you're ready
kubectl get hpa myapp-rc --watch
k get pods 
```
# 4.  *STATEFULSETS:*

A **StatefulSet** is a Kubernetes controller that manages **stateful applications**. Unlike a **ReplicaSet**, which creates identical Pods, a StatefulSet ensures that each Pod has:
1. A **stable, unique identity** (e.g., `pod-0`, `pod-1`, `pod-2`).
2. A **persistent storage volume** that remains even after a Pod is deleted.
3. A **controlled deployment and scaling process** (Pods are started and terminated in order).

## When to Use a StatefulSet?
- Databases (e.g., **MySQL, PostgreSQL, MongoDB, Cassandra**) that require persistent storage.  
- Distributed applications that need **stable network identities** (e.g., **Kafka, Zookeeper, Elasticsearch**).  
- Applications that require **ordered scaling & rolling updates**.

---

## Example 1: Basic StatefulSet for Nginx
This example deploys an **Nginx StatefulSet** where each Pod has a stable hostname and persistent storage.

 `statefulset.yaml`
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: nginx-statefulset
spec:
  serviceName: "nginx"
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
          volumeMounts:
            - name: nginx-storage
              mountPath: /usr/share/nginx/html  # Persistent storage mount
  volumeClaimTemplates:
    - metadata:
        name: nginx-storage
      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 1Gi  # Persistent storage size
```

### What Happens?
- **Pods get stable names:** `nginx-0`, `nginx-1`, `nginx-2`.
- **Each Pod gets a persistent volume** (`nginx-storage`).
- **Scaling happens in order:** `nginx-0` starts first, then `nginx-1`, etc.
- **If a Pod crashes, it is replaced with the same identity**.

---

## Example 2: StatefulSet for MySQL Database
This StatefulSet **deploys a MySQL database** with persistent storage.
 `statefulset.yaml`
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql-statefulset
spec:
  serviceName: "mysql"
  replicas: 3 
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql:5.7
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: "mypassword"
          ports:
            - containerPort: 3306
          volumeMounts:
            - name: mysql-storage
              mountPath: /var/lib/mysql
  volumeClaimTemplates:
    - metadata:
        name: mysql-storage
      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 5Gi  # Persistent storage for MySQL
```

### What Happens?
- **Pods get stable identities:** `mysql-0`, `mysql-1`, `mysql-2`.
- **Each MySQL Pod gets a unique storage volume** (so data is not lost if a Pod is deleted).
- **Pods restart in sequence:** If `mysql-0` is down, Kubernetes restores it first before moving to `mysql-1`.
---

## ** How to Deploy a StatefulSet**
1 **Apply the StatefulSet**  
```bash
kubectl apply -f statefulset.yaml
```

2 **Check the StatefulSet**  
```bash
kubectl get statefulsets
```

3 **Check the Pods**  
```bash
kubectl get pods -l app=nginx
```

4 **Check Persistent Volumes**  
```bash
kubectl get pvc
```

---

## Key Differences: StatefulSet vs. ReplicaSet
| Feature        | StatefulSet | ReplicaSet |
|---------------|------------|------------|
| Pod Identity  | Unique, stable names (e.g., `pod-0`, `pod-1`) | Randomly assigned names |
| Scaling       | Ordered (first `pod-0`, then `pod-1`) | All Pods created/deleted at once |
| Storage       | Persistent storage for each Pod | Shared storage, ephemeral |
| Use Case      | Databases, distributed systems | Stateless applications, web services |

---
## When to Use a StatefulSet?
**Use StatefulSet when your application needs stable identities and persistent storage** (e.g., databases, message queues, distributed systems).  
**Use ReplicaSet/Deployment for stateless apps** where Pod identity doesn’t matter.


# 5. *DEAMONSETS:*

+ Deamonsets are like replicasets which help you run one instance of pods, but it runs one copy of your pod on every node on the cluster.
+ The deamonset ensures that one copy of the pod is always running on every node in the cluster.
+ A use case is if you are deploying a log collecting or monitoring agent.
+ objects like the kube-proxy and network use deamonsets because they have to run on every node.

`deamonset.yaml`
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd-elasticsearch
  namespace: kube-system
  labels:
    k8s-app: fluentd-logging
spec:
  selector:
    matchLabels:
      name: fluentd-elasticsearch
  template:
    metadata:
      labels:
        name: fluentd-elasticsearch
    spec:
      tolerations:
      # These tolerations are to have the daemonset runnable on control plane nodes
      # remove them if your control plane nodes should not run pods
      - key: node-role.kubernetes.io/control-plane
        operator: Exists
        effect: NoSchedule
      - key: node-role.kubernetes.io/master
        operator: Exists
        effect: NoSchedule
      containers:
      - name: fluentd-elasticsearch
        image: quay.io/fluentd_elasticsearch/fluentd:v2.5.2
        resources:
          limits:
            memory: 200Mi
          requests:
            cpu: 100m
            memory: 200Mi
        volumeMounts:
        - name: varlog
          mountPath: /var/log
      #it may be desirable to set a high priority class to ensure that a DaemonSet Pod
      #preempts running Pods
      #priorityClassName: important
      terminationGracePeriodSeconds: 30
      volumes:
      - name: varlog
        hostPath:
          path: /var/log
```
```sh
k create -f <filename>
kubectl get deamonsets 
kubectl describe deamonsets
kubectl get daemonsets --all-namespaces 
```
- How do you get pods to be scheduled on every node?
- one approach is to use the node name to bypass the scheduler and place a pod on a desired node.
  

# Labels and Selectors:
+ Labels are used as filters for ReplicaSet. Labels allow the rs to know what pod in the cluster or nodes 
placed under its management since there could be multiple pods running in the cluster.
+ The template definition section is required in every rs, even for pods that were created before the rs 
+ This is because if the pod fails and is to be recreated, it will need the spec to recreate it

- if you want to scale from 3 to 6 replicas, update the replicas to 6 and run
```sh
 kubectl replace -f <filename>
 kubectl scale --replicas=6 <filename>
 kubectl scale -- replicas replicaset name 
 kubectl edit pod/rs/rc/deploy <podname>
```

# SERVICE DISCOVERY
https://kubernetes.io/docs/concepts/services-networking/service/
+ Pods in a cluster have a unique cluster-wide IP, acting like separate VMs
+ Pods can communicate with all other Pods without NAT
+ Kubernetes service enables communication between various components within and outside the application and between other applications and users.
+ Services expose groups of Pods over a network.

 - A full-stack web app typically has several pods such as frontend pods hosting a web server,
 - the backend hosting the app, and pods hosting a db. Kubernetes service can group all pod groups and provide a single backend to access the pods. you can create another service for all pods running the DB
 - These pods for different applications can therefore be scaled like microservices without impacting the other.
 - A separate SVC for frontend, for backend, and db.
 - For external communication, the Kubernetes node has an IP, the host OS which is in the same network has an IP (private), and the pod has an ip but on a separate network
 - To access the application externally, the k8s service enables communication from pods on nodes

## TYPES:

### Initial Setup: Deploy Nginx

A. **Deployment Configuration:**

 `svc-deploy.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

### 1. ClusterIP Service

**Definition and Use Case:**
- **ClusterIP** is the default Kubernetes service type that assigns a service an internal IP address that can be used to access it from within the cluster. This service is ideal for applications that need to be accessible to other services within the same Kubernetes cluster but not externally.

**Deploy ClusterIP Service:**

1. Create the service definition:

 `svc-deploy.yaml`
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-clusterip
spec:
  type: ClusterIP
  selector:
    app: nginx
  ports:
    - port: 80
  nodePort: 31000

```

Save this as `clusterip-svc.yaml` and deploy it:

```bash
kubectl apply -f clusterip-svc.yaml
```

**Accessing the Application:**
- Run a temporary pod and curl the service:

```bash
kubectl run curl --image=radial/busyboxplus:curl -i --tty
```
or
```bash
kubectl run curl --image=radial/busyboxplus:curl -it --rm -- /bin/sh
```
+ Then in the shell
```bash
curl http://nginx-clusterip
```

**Delete the ClusterIP Service:**

```bash
kubectl delete svc nginx-clusterip
```

### 2. NodePort Service

**Definition and Use Case:**
- **NodePort** is a configuration that allows external traffic to get to a service by mapping a port on each node to the service. It's useful when direct access via Kubernetes cluster IP is not possible.

**Deploy NodePort Service:**

1. Create the service definition:

```yaml
 `svc-deploy.yaml`
apiVersion: v1
kind: Service
metadata:
  name: nginx-nodeport
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30007
```

Save this as `nodeport-service.yaml` and deploy it:

```bash
kubectl apply -f nodeport-service.yaml
```

**Accessing the Application:**

- Get the IP of any node:

```bash
kubectl get nodes -o wide
```

- Access Nginx using the Node IP and NodePort:

```bash
curl http://<Node_IP>:30007
```

**Delete the NodePort Service:**

```bash
kubectl delete svc nginx-nodeport
```

### 3. LoadBalancer Service

**Definition and Use Case:**
- **LoadBalancer** exposes the service externally through a cloud provider’s load balancer.
- This service type is most beneficial when running on a cloud platform that supports automatic load balancers, providing a way to distribute traffic across several instances of the application.

**Deploy LoadBalancer Service:**
# AWS Application Load Balancer (ALB) Installation Guide for EKS

## Prerequisites
- AWS CLI configured with appropriate permissions
- `eksctl` installed
- `kubectl` installed and configured to access your EKS cluster
- Helm installed
- An existing EKS cluster

### 1. Download IAM Policy
Download the IAM policy required for the AWS Load Balancer Controller:
```bash
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.11.0/docs/install/iam_policy.json
```

### 2. Create IAM Policy
Create the IAM policy in your AWS account:
```bash
aws iam create-policy \
  --policy-name AWSLoadBalancerControllerIAMPolicy \
  --policy-document file://iam_policy.json
```

### 3. Associate IAM OIDC Provider
Associate the IAM OIDC provider for your cluster:
```bash
eksctl utils associate-iam-oidc-provider \
  --region eu-central-1 \
  --cluster eks-hilltop-prod \
  --approve
```

### 4. Create IAM Service Account
Create an IAM service account for the controller:
```bash
eksctl create iamserviceaccount \
  --region eu-central-1 \
  --cluster eks-hilltop-prod \
  --namespace kube-system \
  --name aws-load-balancer-controller \
  --role-name AmazonEKSLoadBalancerController \
  --attach-policy-arn arn:aws:iam::0504516180:policy/AWSLoadBalancerControllerIAMPolicy \
  --approve
```

### 5. Add Helm Repository
Add the EKS Helm repository:
```bash
helm repo add eks https://aws.github.io/eks-charts
helm repo update
```

### 6. Install AWS Load Balancer Controller
Install the controller using Helm:
```bash
helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system \
  --set clusterName=eks-hilltop-prod \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller \
  --set region=eu-central-1 \
  --set vpcId=vpc-05bd534e99630eca3 \
  --version 1.13.0
```

### 7. Apply CRDs
Apply the Custom Resource Definitions:
```bash
curl -O https://raw.githubusercontent.com/aws/eks-charts/master/stable/aws-load-balancer-controller/crds/crds.yaml
kubectl apply -f crds.yaml
```

### 8. Verify Installation
Verify the controller is running:
```bash
kubectl get deployment -n kube-system aws-load-balancer-controller
```

## Deploying a Sample Application with LoadBalancer Service

### 1. Create a Sample Deployment
Create a deployment file `sample-deployment.yaml`:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: color-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: color-app
  template:
    metadata:
      labels:
        app: color-app
    spec:
      containers:
      - name: color-app
        image: hilltopconsultancy/colorapp:orange
        ports:
        - containerPort: 8080
```

Apply the deployment:
```bash
kubectl apply -f sample-deployment.yaml
```

### 2. Create a LoadBalancer Service
Create a service file `loadbalancer-service.yaml`:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: color-service
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-type: external
    service.beta.kubernetes.io/aws-load-balancer-nlb-target-type: ip
    service.beta.kubernetes.io/aws-load-balancer-scheme: internet-facing
spec:
  selector:
    app: color-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: LoadBalancer
```

Apply the service:
```bash
kubectl apply -f loadbalancer-service.yaml
```

## Testing the Load Balancer

### 1. Check Service Status
```bash
kubectl get svc sample-service
```

Wait until the `EXTERNAL-IP` column shows a DNS name (this may take a few minutes).

### 2. Access the Application
Once the LoadBalancer is provisioned, you can access it using the provided DNS name:
```bash
curl http://<LOAD_BALANCER_DNS>
```

You should see the default COLOR welcome page.

### 3. Verify Load Balancer in AWS Console
1. Go to the AWS Management Console
2. Navigate to EC2 > Load Balancers
3. You should see a new load balancer created for your service

## Cleanup

To remove the resources:
```bash
kubectl delete -f loadbalancer-service.yaml
kubectl delete -f sample-deployment.yaml
helm uninstall aws-load-balancer-controller -n kube-system
```

## Troubleshooting

If the LoadBalancer is not being created:
1. Check controller logs:
   ```bash
   kubectl logs -n kube-system deployment/aws-load-balancer-controller
   ```
2. Verify IAM permissions
3. Check for errors in the service events:
   ```bash
   kubectl describe svc sample-service
   ```
---
# **AWS ALB Ingress with TLS: Complete Step-by-Step Guide**

## **1. Definitions**
### **What is an Ingress?**
An Ingress is a Kubernetes resource that manages external access to HTTP/HTTPS services in a cluster. It provides:
- **Load balancing** (distributes traffic to pods)
- **SSL/TLS termination** (HTTPS support)
- **Host-based routing** (`app1.domain.com`, `app2.domain.com`)
- **Path-based routing** (`/api`, `/web`)

### **What is an Ingress Controller?**
The **AWS Load Balancer Controller** is required to implement Ingress rules by provisioning an Application Load Balancer (ALB). We already installed this in previous steps(alb).

---

## **2. AWS Console Setup (Certificate + Domain)**

### **Step 1: Create TLS Certificate in AWS ACM**
1. Go to **AWS ACM Console** (https://console.aws.amazon.com/acm)
2. Click **Request certificate**
3. Select **Public certificate**
4. Add domain names:
   - **Fully qualified Domain:** `colorapp.hilltopdevops.com`
   - (Optional) Add wildcard: `*.hilltopdevops.com`
5. Choose **DNS validation**
6. Click **Request**
7. In certificate list, select your new certificate
8. Click **Create records in Route53** (AWS will auto-create validation records)
9. Wait 5-30 minutes for status to change from **Pending validation** → **Issued**

---

## **3. Kubernetes Deployment**

### **Step 3: Deploy Application**
```yaml
# color-app-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: color-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: color-app
  template:
    metadata:
      labels:
        app: color-app
    spec:
      containers:
      - name: color-app
        image: hilltopconsultancy/colorapp:orange
        ports:
        - containerPort: 8080
        env:
        - name: COLOR
          value: "orange"
```

```bash
kubectl apply -f color-app-deployment.yaml
```

### **Step 4: Create ClusterIP Service**
```yaml
# color-app-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: color-app-service
spec:
  type: ClusterIP
  selector:
    app: color-app
  ports:
    - port: 80
      targetPort: 8080
```

```bash
kubectl apply -f color-app-service.yaml
```

### **Step 5: Create Ingress Resource**
```yaml
# color-app-ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: color-app-ingress
  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
    alb.ingress.kubernetes.io/certificate-arn: arn:aws:acm:eu-central-1:050451396180:certificate/YOUR_CERT_ID
    alb.ingress.kubernetes.io/listen-ports: '[{"HTTPS":443}]'
    alb.ingress.kubernetes.io/ssl-redirect: '443'
spec:
  ingressClassName: alb
  rules:
  - host: colorapp.hilltopdevops.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: color-app-service
            port:
              number: 80
```

```bash
kubectl apply -f color-app-ingress.yaml
```

---
### **Step 2: Create Subdomain in Route53**
1. Go to **Route53 Console** (https://console.aws.amazon.com/route53)
2. Select your hosted zone: **hilltopdevops.com**
3. Click **Create record**
4. Configure:
   - **Record name:** `colorapp`
   - **Record type:** `A`
   - **Alias:** Enable
   - **Route traffic to:** 
     - Select **Alias to Application and Classic Load Balancer**
     - Choose region: **eu-north-1**
     - (We'll select the ALB after creating the Ingress)
5. Click **Create records**

---
## **4. Final Configuration in AWS Console**

### **Step 6: Update Route53 ALB Target**
1. Get ALB DNS name:
   ```bash
   kubectl get ingress color-app-ingress -o jsonpath='{.status.loadBalancer.ingress[0].hostname}'
   ```
   Example output: `k8s-default-colorapp-ingress-xxxx.elb.eu-central-1.amazonaws.com`

2. Go back to **Route53 Console** → **hilltopdevops.com**
3. Edit the `colorapp` A record:
   - Update **Alias target** to your ALB DNS name
   - Save changes

---

## **5. Verification**

### **Step 7: Test Your Setup**
1. Wait 2-5 minutes for DNS propagation
2. Access your application:
   ```bash
   curl -v https://colorapp.hilltopdevops.com
   ```
3. Verify:
   - HTTPS works (padlock icon in browser)
   - HTTP → HTTPS redirect works:
     ```bash
     curl -v http://colorapp.hilltopdevops.com
     ```
   - Should return `301 Moved Permanently` to HTTPS

---

## **Troubleshooting**
- **Certificate not validating?**
  - Check ACM console for validation status
  - Verify CNAME records exist in Route53
- **ALB not created?**
  ```bash
  kubectl describe ingress color-app-ingress
  ```
  Look for errors in events
- **DNS not resolving?**
  ```bash
  dig colorapp.hilltopdevops.com
  ```
  Should return ALB DNS name

---

## **Cleanup**
```bash
kubectl delete -f color-app-ingress.yaml
kubectl delete -f color-app-service.yaml
kubectl delete -f color-app-deployment.yaml
# Delete ACM certificate and Route53 records via AWS Console if no longer needed
```

---
# STORAGE:
## VOLUMES:
Files in a container are ephemeral and they will be lost once the container fails or is stopped
The container is recreated in a clean state and all volumes I lost
+ Therefore persisting volumes are essential in k8s as they persistent volumes exist beyond the lifetime of a pod.
+ These persistent volumes can be a directory that can be used by Pods or shared by containers running in a Pod

# Kubernetes ConfigMaps Guide

Based on [Kubernetes Official Documentation](https://kubernetes.io/docs/concepts/configuration/configmap/)

## Overview
A ConfigMap is an API object used to store non-confidential data in key-value pairs. Pods can consume ConfigMaps as environment variables, command-line arguments, or as configuration files in a volume. ConfigMaps allow you to decouple environment-specific configuration from your container images, making your applications easily portable.

## Key Features
- Stores configuration data separately from application code
- Can be mounted as data volumes or exposed as environment variables
- Namespace-scoped (exist within a single namespace)
- Can be updated without rebuilding images
- Supports UTF-8 strings up to 1MiB in size

## Creating ConfigMaps

### 1. From Literal Values
```bash
kubectl create configmap app-config \
  --from-literal=COLOR=red \
  --from-literal=ENVIRONMENT=production \
  --from-literal=LOG_LEVEL=debug
```

### 2. From YAML File
Create `app-config.yaml`:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  COLOR: red
  ENVIRONMENT: production
  LOG_LEVEL: debug
  UI_PROPERTIES_FILE_NAME: ui.properties
```

Apply the ConfigMap:
```bash
kubectl apply -f app-config.yaml
```

## Using ConfigMaps in Pods

### Example 1: Using All Environment Variables from ConfigMap

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: colorapp-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: colorapp
  template:
    metadata:
      labels:
        app: colorapp
    spec:
      containers:
      - name: colorapp
        image: hilltopconsultancy/colorapp:orange
        ports:
        - containerPort: 8080
        envFrom:
          - configMapRef:
              name: app-config
```

### Example 2: Using Specific Environment Variables from ConfigMap

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: colorapp-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: colorapp
  template:
    metadata:
      labels:
        app: colorapp
    spec:
      containers:
      - name: colorapp
        image: hilltopconsultancy/colorapp:orange
        ports:
        - containerPort: 8080
        env:
          - name: COLOR
            valueFrom:
              configMapKeyRef:
                name: app-config
                key: COLOR
          - name: LOG_LEVEL
            valueFrom:
              configMapKeyRef:
                name: app-config
                key: LOG_LEVEL
```

### Example 3: Using ConfigMap as Volume

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: colorapp-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: colorapp
  template:
    metadata:
      labels:
        app: colorapp
    spec:
      containers:
      - name: colorapp
        image: hilltopconsultancy/colorapp:orange
        ports:
        - containerPort: 8080
        volumeMounts:
        - name: config-volume
          mountPath: /etc/config
      volumes:
      - name: config-volume
        configMap:
          name: app-config
```

## Complete Deployment Example

1. Create the ConfigMap (`app-config.yaml`):
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  COLOR: red
  ENVIRONMENT: production
  LOG_LEVEL: debug
  APP_NAME: ColorApp
```

2. Create the Deployment (`colorapp-deployment.yaml`):
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: colorapp-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: colorapp
  template:
    metadata:
      labels:
        app: colorapp
    spec:
      containers:
      - name: colorapp
        image: hilltopconsultancy/colorapp:orange
        ports:
        - containerPort: 8080
        envFrom:
          - configMapRef:
              name: app-config
```

3. Create the Service (`colorapp-service.yaml`):
```yaml
apiVersion: v1
kind: Service
metadata:
  name: colorapp-service
spec:
  type: NodePort
  selector:
    app: colorapp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
      nodePort: 30080
```

## Deploying the Application

```bash
kubectl apply -f app-config.yaml
kubectl apply -f colorapp-deployment.yaml
kubectl apply -f colorapp-service.yaml
```

## Verifying Configuration

1. Check environment variables in a pod:
```bash
kubectl exec -it pod/colorapp -- sh -c "printenv"
```

2. Check the application logs:
```bash
kubectl logs -l app=colorapp
```

## Updating ConfigMaps

1. Edit the ConfigMap:
```bash
kubectl edit configmap app-config
```

2. Update the COLOR value to a different color (e.g., blue)

3. Restart the pods to pick up changes:
```bash
kubectl rollout restart deployment colorapp-deployment
```

## Best Practices
- ConfigMaps should be used for non-sensitive configuration only
- ConfigMaps are namespace-scoped
- Consider using immutable ConfigMaps for configurations that don't need updates
- The total size of a ConfigMap must be less than 1MiB
- When mounted as volumes, ConfigMap updates are eventually consistent

## Cleanup

```bash
kubectl delete -f colorapp-service.yaml
kubectl delete -f colorapp-deployment.yaml
kubectl delete -f app-config.yaml
```
### 2. *emptyDir:*

+ An emptyDir volume is created when the Pod is assigned to a node, the emptyDir volume is initially empty
+ All containers in the Pod can read and write the same files in the emptyDir volume, 
+ that volume can be mounted at the same or different paths in each container. 
+ When a Pod is removed from a node for any reason, the data in the emptyDir is deleted permanently.

```sh
apiVersion: v1
kind: Pod
metadata:
  name: test-pd
spec:
  containers:
  - image: registry.k8s.io/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /cache
      name: cache-volume
  volumes:
  - name: cache-volume
    emptyDir:
      sizeLimit: 500Mi
```

### 3. *hostPath:*

+ A hostPath volume mounts a file or directory from the host node's filesystem into your Pod. #It is risky
+ This is not something that most Pods will need, but it offers a powerful escape hatch for some applications.

```sh
# This manifest mounts /data/foo on the host as /foo inside the
# single container that runs within the hostpath-example-linux Pod.
# The mount into the container is read-only.
apiVersion: v1
kind: Pod
metadata:
  name: hostpath-example-linux
spec:
  os: { name: linux }
  nodeSelector:
    kubernetes.io/os: linux
  containers:
  - name: example-container
    image: registry.k8s.io/test-webserver
    volumeMounts:
    - mountPath: /foo
      name: example-volume
      readOnly: true
  volumes:
  - name: example-volume
    # mount /data/foo, but only if that directory already exists
    hostPath:
      path: /data/foo # directory location on host
      type: Directory # This field is optional
```
### 4. *SECRETS:*

Secrets just like configmaps are used to store configuration data which can be injected into an object in k8s.
Unlike configmaps, secrets store sensitive data such as passwords and keys in an encoded manner.
You can create a secret imperatively by using:
```sh
  kubectl create secret generic <secretName> app-secret --from-literal=DB_Host=mysql   \
                                                        --from-literal=DB_User=root
```
    You can ref the secret from a file using the --from-file=app-secret
    For a declarative approach
```sh
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
data:
  DB_Host: mysql
  DB_User: root 
  DB_Password: password
```
It is however not advisable to pass your secrets in plain text as if defeats the entire purpose.
To convert the data from plaintext to an encoded format, on a Linux system, use the  
```sh 
echo -n 'mysql' | base64
echo -n 'root' | base64
echo -n 'password' | base64
```
```sh
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
data:
  DB_Host: sjhdvdv=
  DB_User: fnvjsf== 
  DB_Password: sffvnhri
```
copy the corresponding encoded values and replace and inject them into the file.
```sh
kubectl get secrets app-secret
kubectl describe secrets
kubectl get secrets app-secret -o yaml
```
+ to decode encoded values use the  
```sh
echo -n 'djvfjdo=' | base64 --decode
```
+ To inject encoded values into the pod object, use the 
```sh
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    ports:
      - containerPort: 8080
    envFrom:
      - secretRef:
        name: app-secret
```
+ Secrets are not encrypted but rather encoded and can be decoded using the same method. 
+ Therefore, do not upload your secret files to the GitHub repo.

You can enable encryption at rest:
```sh
kubectl get secrets --all-namespaces -o jason | kubectl replace -f -
```
https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/

## i. *PersistentVolume (PV)*

+ A PersistentVolume (PV) is a piece of storage in the cluster that has been provisioned by an administrator
+ PVs are volume plugins like Volumes, but have a lifecycle independent of any individual Pod that uses the PV
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: task-pv-volume
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```
## ii. *PersistentVolumeClaim (PVC)*
+ A PersistentVolumeClaim (PVC) is a request for storage by a user. 
+ Pods consume node resources and PVCs consume PV resources.
+ While Pods can request specific levels of resources (CPU and Memory), Claims can request specific size and access modes (e.g., they can be mounted ReadWriteOnce, ReadOnlyMany, ReadWriteMany, or ReadWriteOncePod,)
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: task-pv-claim
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: task-pv-pod
spec:
  volumes:
    - name: task-pv-storage
      persistentVolumeClaim:
        claimName: task-pv-claim
  containers:
    - name: task-pv-container
      image: nginx
      ports:
        - containerPort: 80
          name: "http-server"
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: task-pv-storage
```
## Volume Lifecycle:
The reclaim policy for a PersistentVolume tells the cluster what to do with the volume after it has been released of its claim
+ Recycle
+ Retain
+ Delete


### NAMESPACES:

  A namespace is simply a distinct working area in k8s where a defined set of resources rules and users can  
  be assigned to a namespace. 
- By default, a k8s cluster comes with a default namespace. Here, a user can provision resources.
- the subsystem namespace is also created by default for a set of pods and services for its functioning.
- The kubepublic is also created to host resources that are made available to the public.
- Within a cluster, you can create different namespaces for different projects and allocate resources to that namespace.
- Resources from the same namespaces can refer to each other by their names,
- they can also communicate with resources from another namespace by their names and append their namespace.
eg msql.connect("db-service.dev.svc.cluster.local")
```sh
kubectl get pods > will list only pods in the default namespace
kubectl get pods --namespace=kubesystem

kubectl apply -f <filename>  ==> will create object in the default namespace
kubectl create -f <filename> --namespace=kubesystem  ==> will create object in the kubesystem namespace
```

### Context
A **context** 
+ in Kubernetes is a configuration that combines a cluster, a user, and a namespace into a single, reusable entity.
+ Contexts are defined in the kubeconfig file, which is typically located at `~/.kube/config`.

- **Cluster**: Refers to the specific Kubernetes cluster.
- **User**: Specifies the credentials used to access the cluster.
- **Namespace**: Sets a default namespace to use for kubectl commands within the context.

+ By switching contexts, you change the cluster, user, and/or namespace that kubectl interacts with by default.

**Commands to manage contexts:**
- List all contexts:
  ```sh
  kubectl config get-contexts
  ```
- Display the current context:
  ```sh
  kubectl config current-context
  ```
- Switch to a different context:
  ```sh
  kubectl config use-context <context-name>
  ```

### Namespace
A **namespace** 
+ is a way to divide cluster resources between multiple users via virtual clusters. Namespaces provide a mechanism for isolating groups of resources within a single cluster.
+ They are especially useful in larger environments where teams or projects need their own spaces for resources.

**Key points about namespaces:**
- Namespaces are a way to organize and isolate resources within a cluster.
- Namespaces do not provide complete isolation; they are intended for logical separation rather than security.
- Each resource created in Kubernetes belongs to a namespace, except for cluster-wide resources (like nodes and persistent volumes) which are not namespaced.

**Commands to manage namespaces:**
- List all namespaces:
  ```sh
  kubectl get namespaces
  ```
- Create a new namespace:
  ```sh
  kubectl create namespace <namespace-name>
  ```
- Set the default namespace for the current context:
  ```sh
  kubectl config set-context --current --namespace=<namespace-name>
  ```

### Example
Consider a scenario where you have two contexts defined in your kubeconfig:
- `dev-context` pointing to the development cluster with namespace `development`.
- `prod-context` pointing to the production cluster with namespace `production`.

Switching contexts changes both the cluster and namespace:
```sh
kubectl config use-context dev-context
kubectl config use-context prod-context
```

Setting a namespace within a context only changes the namespace for that context:
```sh
kubectl config set-context --current --namespace=staging
```

 [Kubernetes official documentation on using contexts](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/#context).

To ensure that your resources are always created in a specific namespace, add the namespace block in the resources
definition file
```yaml
apiVersion: v1
Kind: Service
metadata: 
  name: backend
  namespace: dev  # This resource will always be created in the dev namespace, and will create the ns if it didn't exist
spec:
  type: LoadBalancer
  ports:
    - targetPort: 80  # (port on Pod). it will assume port if not specified
      port: 80 # port on service. this is a mandatory field
  selector:
    app: myapp # This is the label that was used in the deployment metadata section
    type: backend
```

```yaml
apiVersion: v1
Kind: NameSpace
metadata:
  name: dev
```
OR 
 kubectl create namespace dev
 kubectl get ns 

to set a namespace as the default namespace so that you don't always have to pass the NameSpace command, you need to set
set the namespace in the current context

kubectl config set-context $(kubectl config current-context) --namespace=dev
contexts are used to manage all resources in clusters from a single system 

To view resources in all NameSpaces use the 
kubectl get pods --all-namespaces

### Resource Quota:
To set a limit for resources in a namespace, create a resource-quota object in
```yaml
apiVersion: v1
Kind: ResourceQuota
metadata:
  name: compute-quota
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 5Gi
    limits.cpu: "10"
    limits.memory: 10Gi
```
kubectl apply -f <filename>

### Declarative and Imperative :
  these are different approaches to creating and managing infrastructure in IaC.
- The Imperative approach is giving just the required infrastructure and the resource figures out the steps involved.
eg kubectl create deployment nginx --image nginx
    kubectl set image deployment nginx nginx=nginx:1.18
    kubectl replace -f nginx.yaml

    it creates resources quickly but is difficult to edit or understand what is involved.

- in the Declarative approach, a step-by-step approach through writing configuration files
   Here we can write configuration files 
   Here, changes can be made as well as the resources can be versioned.
   resources can also be edited in the running state using the kubectl edit command
   The best practice is always to edit the configuration file and run the replace command rather than using the edit command.

   when you use the kubectl apply command, it will create objects that do not exist, and when you want to update the object,
   edit the yml file and run kubectl apply again and the object will pick up the latest changes in the file.
```sh
kubectl run --image=nginx nginx 
kubectl create deployment --image=nginx nginx 
kubectl edit deployment nginx 
kubectl scale deployment nginx --replicas=5
kubectl set image deployment nginx nginx=nginxv
```
```sh
kubectl run nginx --image=nginx --dry-run=client -o yaml    > nginx-deployment.yaml
kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml 
kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml
k create deploy redis-deploy --image=redis --replicas=2 --namespace=dev-ns
kubectl run httpd --image=httpd:alpine --port=80 --expose   #will create pod and service
```
when you run a kubectl apply command, if the object stated in the file does not exist, it is created.
another live object configuration is created with additional fields and can be viewed using the  
- kubectl edit/describe object <objectName>
there is the last applied file that provides details about the last image of the live configuration.


### SCHEDULING:

- there is a builtin scheduler in the cluster control-plane, that scans through nodes in the cluster and schedules
  pods on nodes based on several factors such as resources,
- But if you want to override and schedule your pods on specific nodes for some reason, you can do that by
  specifying the nodeName in the pod definition file.
- If a scheduler does not exist in the cluster, the pod will continually be pending.
- If you need a pod to run on a specific node, declare it at the time of creation.
- Kubernetes does not allow node modification after the pod has already been created.
- It can only be modified by creating a binding object setting the target to the NodeName and then sending a post 
 request to the pod's binding API.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  -  image: nginx
     name: nginx
  nodeName: controlplane
```
## RESOURCE REQUIREMENTS:

- every pod requires a set of resources to run.
when a pod is placed on a node, it consumes the resources on that node.
the scheduler determines the node a pod will be scheduled on based on resource availability.
if nodes have insufficient resources, the scheduler keeps the pod in a pending state.
- You can specify the resource requested by  a pod to run.
the scheduler will look for a node that has that resource specification and place that pod on it.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    resources:
      requests:
        memory: "4Gi"
        cpu: 2
      limits:
        memory: 
        cpu: 
  nodeName: controlplane
```

- When a pod tries to exceed resources out of its limits, the system throttles the containers so that it doesn't
  use more than its limit,
- as for memory, a container can use more memory resources than its limit. but a pod cannot
- By default, k8s does not have a request and limit set, therefore resources can consume as much as they need.
- One pod can consume more and prevent others from running.
- When a CPU limit is set without request, k8s sets the request to the same as the limit
- When CPU requests and limits are set, then they stay within the range. But if one pod isn't consuming resources,
  then it is securing resources that other pods could use.
- When requests are set without limits, any pods can consume as many CPUs are required and when a pod needs more  
  resources, it has a guaranteed resource but without a limit. Make sure all pods have requests set.

LimitRanges as objects can be used to ensure that every pod created has some default values at the namespace level.
You can set it for both CPU and memory at the ns level. all pods will assume that standard.
ResourceQuota can also be used to set resource limits at the level of the NameSpace.

## QUALITY OF SERVICE IN KUBERNETES:

Kubernetes offers three levels of Quality of Service:

+ 1. **BestEffort:**
   - Pods with BestEffort QoS are not guaranteed any specific amount of resources.
   - They are scheduled onto nodes based on availability, and they can use whatever resources are available at that time.
   - These pods are the first to be evicted if resources become scarce.

+ 2. **Burstable:**
   - Pods with Burstable QoS are guaranteed a minimum amount of CPU and memory.
   - These pods can burst beyond their guaranteed minimum if the resources are available.
   - If other pods on the node need resources, Burstable QoS pods might be limited in their burst capacity.

+ 3. **Guaranteed:**
   - Pods with Guaranteed QoS are guaranteed a specific amount of CPU and memory.
   - These pods are not allowed to exceed the resources they have been allocated.
   - Kubernetes tries to ensure that nodes have enough available resources to meet the guaranteed requirements.

Kubernetes determines the QoS level of pods based on the resource requests and limits specified in the pod's configuration:

- **Resource Requests:** The minimum amount of resources a pod requires to run. 
    These requests are used by the scheduler to make placement decisions.
- **Resource Limits:** The maximum amount of resources a pod is allowed to use.
    Exceeding these limits could lead to throttling or pod termination.

Kubernetes uses the relationship between requests and limits to categorize pods into different QoS classes. 
The actual QoS class assigned to a pod depends on how its requests and limits are set:

- **BestEffort:** Pods with no resource requests or limits.
- **Burstable:** Pods with resource requests, but without memory limits or with memory limits lower than their requests.
- **Guaranteed:** Pods with both CPU and memory limits set to be higher than or equal to their resource requests.

Setting appropriate resource requests and limits for pods is crucial for efficient resource allocation and QoS management 
within a Kubernetes cluster. Properly configured QoS levels help ensure that critical workloads are prioritized 
and that the cluster operates smoothly without resource contention issues.

## STATIC PODS:

- Without the control-plane which contains the API server, you can store your configuration files at the path 
  /etc/kubernetes/manifest,.
- kubernetes frequently visit this directory and any manifest file here to create pods will create the pod.
- it can create only pods, another object will need the controlplane
- The static pod folder can be any directory, but the path is set in the kubelet.service file in the kubeconfig.yaml
- Static pods can be viewed by running the docker ps command.
- The kubelet can create resources in k8s either by using configuration files or by listening to the kube API endpoints
  from the control controlplane.
- If you run the k get pods command, it will also list the static pods. This is blc a mirror of the static pods that are  
- created in the kubeapi but like other pods, can not be edited, except in the pod definition files.
- A use case for static pods is when you want to install components of the k8s control plane on every node, then you start 
  by installing the kubelet service and then create pod definition files that use docker images of all other 
  components of the controlplane and place them in the etc/kubernetes/manifest dir

kubectl get pods -n kube-system

## Multiple Schedulers:

You can deploy an optional scheduler added to the custom scheduler and configure it to schedule specific pods.
you can use a pod definition file or wget the scheduler binary and remane the scheduler to a different name.
to make sure that your object to be created is managed by that scheduler, you can add a scheduler option under
the spec section and pass the name of the scheduler.

kubectl logs object objectname

scheduling queu ===> priority sort
filtering       ===> NodeName/NodeUnschedulable/NodeReourceFit
scoring         ===> NodeReourceFit/ImageLocality
binding         ===> Defaultbinding


## APPLICATION LIFECYCLE MANAGEMENT:

## 1. *Rolling Update and rollback:*
  when you first create a deployment, it triggers a rollout which can be rev 1
  later when the image is updated, a new rollout is made called rev2
  to see the status of the rollout, run the  
  kubectl rollout status deployment/<deploymentname>

  to see the revision and the rollout history, run the
   kubectl rollout history deployment/myapp-deployment

there are two types of deployment strategies.

## 2. *RECREATE:*
- You can delete the existing deployment and then make a new deployment with the newer version
- This will lead to app downtime. this is not the k8s default strategy.
  
## 3. ROLLING UPDATE:
- Here, pods replicas are progressively destroyed and replaced with newer pods to ensure there is no downtime 
- This is the k8s default update strategy.

update can be done by changing the version of the image, replicas  
kubectl apply -f <filename>

it is advisable to manually edit the definition file than using the imperative approach because with an Imperative,
changes will not be saved in the definition file.

to rollback run the kubectl rollout undo deployment/myapp-deployment
```sh
kubectl apply -f deployment
kubectl get deployment
kubectl rollout status deployment/myapp-deployment
kubectl rollout history deployment/myapp-deployment
kubectl rollout undo deployment/app 
```

 # SECURITY:
Security in Kubernetes is mainly in the area of 
* Securing the configurable cluster components
* Securing the applications that run in the cluster

![4c](https://github.com/CHAFAH/DevOps_Setup/assets/125821852/eee2926b-6e4a-4e4a-ba70-36b6c2cc4fff)

 ## Authentication (Login)
+ All Kubernetes clusters have two categories of users: service accounts managed by Kubernetes, and normal users.
+ You can authenticate into the cluster by either
```sh
kubectl create ns dev
kubectl create sa development --ns dev
kubectl create role dev --ns dev
kubectl create roleBinding dev --role=dev --serviceAccount=development:dev
```

## HOW TO ISSUE A CERTIFICATE TO A USER IN THE CLUSTER:
+ A few steps are required to get a normal user to be able to authenticate and invoke an API. 
+ First, this user must have a certificate issued by the Kubernetes cluster, and then present that certificate to the Kubernetes API.

### 1. Create private key
The following scripts show how to generate PKI private key and CSR. It is important to set CN and O attributes of the CSR.
 CN is the name of the user and O is the group that this user will belong to. You can refer to RBAC for standard groups.
```sh
openssl genrsa -out prince.key 2048
touch /root/.rnd
chmod 600 /root/.rnd
openssl req -new -key prince.key -out prince.csr -subj "/CN=prince"
```
### 2. Create a CertificateSigningRequest
+ Create a CertificateSigningRequest and submit it to a Kubernetes Cluster via kubectl. Below is a script to generate the CertificateSigningRequest.
```sh
cat <<EOF | kubectl apply -f -
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: prince
spec:
  request: #(base64 encoded value of the CSR file content)
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 86400  # one day
  usages:
  - client auth
EOF
```
#Some points to note:

+ usages has to be 'client auth'

+ expirationSeconds could be made longer (i.e. 864000 for ten days) or shorter (i.e. 3600 for one hour)

+ request is the base64 encoded value of the CSR file content. You can get the content using this command:
```sh
cat prince.csr | base64 | tr -d "\n"
```
### 3. Approve the CertificateSigningRequest
+ Use kubectl to create a CSR and approve it.

+ Get the list of CSRs:
```sh
kubectl get csr
```
### 4. Approve the CSR:
```sh
kubectl certificate approve prince
```
### 5. Get the certificate

+ Retrieve the certificate from the CSR:
```sh
kubectl get csr/prince -o yaml
```
+ The certificate value is in Base64-encoded format under status.certificate.

### 6. Export the issued certificate from the CertificateSigningRequest.
```sh
kubectl get csr prince -o jsonpath='{.status.certificate}'| base64 -d > prince.crt
```


### 7. Create Role and RoleBinding
+ With the certificate created it is time to define the Role and RoleBinding for this user to access Kubernetes cluster resources.

+ This is a sample command to create a Role for this new user:
```sh
cat <<EOF | kubectl apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: devops
  name: devops-role
rules:
- apiGroups: [""]
  resources: ["pods", "deployment" "secrets" "services"]
  verbs: ["get", "list", "watch"]
EOF
```
+ This is a sample command to create a RoleBinding for this new user:
```sh
cat <<EOF | kubectl apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: devops-role-binding
  namespace: devops
subjects:
- kind: User
  name: prince
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: devops-role
  apiGroup: rbac.authorization.k8s.io
EOF
```
### 8. Add to kubeconfig
+ The last step is to add this user into the kubeconfig file.

+ First, you need to add new credentials:
```sh
kubectl config set-credentials prince --client-key=prince.key --client-certificate=prince.crt --embed-certs=true
```
### 9. Then, you need to add the context:
```sh
kubectl config set-context prince --cluster=kubernetes --user=prince
```
+ To test it, change the context to prince:
```sh
kubectl config use-context prince
```
+ See the current context
```sh
kubectl config current-context
```
```sh
kubectl auth can-i list pods --namespace devops
```
+ If you install Kubernetes with kubeadm, most certificates are stored in `/etc/kubernetes/pki`.
  https://kubernetes.io/docs/setup/best-practices/certificates/
+ [You can manage Kubeadm certificates here](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-certs/)
  
 ## Authorization
 + After being authenticated into a k8s cluster, all API requests made by an authenticated user in the cluster must be authorized by the API server against all objects and processes
 + Permissions in the cluster to make API calls are denied by default
 + The API request attributes being authorized are either the user, group, resource, verb, namespace path, etc
 + 
### Authorization Modes
## 1. [Attribut Based Access Control ABAC](https://kubernetes.io/docs/reference/access-authn-authz/abac/)
+ Rights are granted to users through the use of policies that combine attributes.
+ The policies can use any type of attribute (user, resource, object, environment, etc)
+ Examples
+ Mysuer can do anything to all resources:
```sh
{"apiVersion": "abac.authorization.kubernetes.io/v1beta1", "kind": "Policy", "spec": {"user": "prince", "namespace": "*", "resource": "*", "apiGroup": "*"}}
```
+ Bob can just read pods in namespace "projectCaribou":
```sh
{"apiVersion": "abac.authorization.kubernetes.io/v1beta1", "kind": "Policy", "spec": {"user": "prince", "namespace": "projectCaribou", "resource": "pods", "readonly": true}}
```
## 2. [Role Based Access Control RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)

+ This is a way of authorization where users in the cluster are permitted to perform actions based on their role in the company
+ This generally follows the principles of least privilege
+ To enable RBAC, start the apiserver with --authorization-mode=RBAC.
+ The RBAC permissions are granted in the form of Roles and ClsuterRoles which are additive i.e, there is not deny
+ A Role always sets permissions within a particular namespace; when you create a Role, you have to specify the namespace it belongs in.
+ ClusterRole, by contrast, is a non-namespaced resource.
+ A RoleBinding or ClusterRoleBinding is used to attach the ClusterRole or Role to the new user.

## *Role and RoleBinding Example:*
+ A role gives a user access to API Resources within a specific namespace
```sh
cat <<EOF | kubectl apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: dev
  name: pod-reader
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
EOF
```

+ This role binding allows "prince" to read pods in the "default" namespace.
+ You need to already have a Role named "pod-reader" in that namespace.
```sh
cat <<EOF | kubectl apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: dev
subjects:
# You can specify more than one "subject"
- kind: User
  name: prince # "name" is case sensitive
  apiGroup: rbac.authorization.k8s.io
roleRef:
  # "roleRef" specifies the binding to a Role / ClusterRole
  kind: Role #this must be Role or ClusterRole
  name: pod-reader # This must match the name of the Role or ClusterRole you wish to bind to
  apiGroup: rbac.authorization.k8s.io
```

## *ClusterRole and ClusterRoleBinding Example:*
+ A Cluster role grants a user access to peform specific functions with the entire cluser.
+ Even though they are action bound, they are not namespace bound
  
```sh
cat <<EOF | kubectl apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  # "namespace" omitted since ClusterRoles are not namespaced
  name: secret-reader
rules:
- apiGroups: [""]
  #
  # at the HTTP level, the name of the resource for accessing Secret
  # objects is "secrets"
  resources: ["secrets"]
  verbs: ["get", "watch", "list"]
```
```sh
cat <<EOF | kubectl apply -f -
apiVersion: rbac.authorization.k8s.io/v1
# This cluster role binding allows anyone in the "manager" group to read secrets in any namespace.
kind: ClusterRoleBinding
metadata:
  name: read-secrets-global
subjects:
- kind: Group
  name: manager # Name is case sensitive
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: secret-reader
  apiGroup: rbac.authorization.k8s.io
```

# NETWORKING:
  
+ Linux Networking Basics:
  - Switching:
    to establish communication between two or more devices, an interface on each host is needed to connect them to the switch.
    Use the ip link command to see the interface.
    Assign an IP to the devices from the network 
ip addr add 192.168.1.10/24 dev eth0  

ping 192.168.1.10
- For a system in one network to communicate with another device in a different network, a router is needed.
- A router helps to connect networks.
- To add a route, check if a route exists as well as it must be configured in both networks if they intend to communicate 
route  
ip route add 192.168.2.0/24 via 192.168.1.1 

- The router gets two IPs  assigned to it to communicate between the two networks.
- A route
- For devices in these networks to communicate to the internet, add a new routing on the router for both networks
- You can also set a default router that routes r=traffic through that IP to the internet

## DNS:
To communicate with different devices and different networks, instead of calling their IP addresses, you can assign a name   
to that IP which is called name resolution.

cat >> /etc/hosts
191.168.1.11   db

run the hostname command to see the name set to the device.
This DNS set on a system for another system is only known to the system and not to all other devices and networks  
Every run a command like an ssh db,   curl https//:db, it will look into the /etc/hosts file
because it grew complex and difficult to manage, it was moved to a DNS server where all entries can be added to and  
then when configured on the host, it will resolve the name from the entry in the DNS server.
- When an entry is added both on the host file and in the DNS server, the system refers first to the host file.
- the order can be changed to resolve the DNS server before the host file.

the .com   .org  .edu  .net .io 
These are top-level domains and they represent the intent of the websites.
www.google.com

www: is a subdomain # can have multiple subdomains
. : root
.com : top-level domain 

when a request is sent to apps.google.com
Org_DNS
root_DNS
.com_DNS
google_DNS
- Each time a request is sent, it goes through all these stages to get to the service. 
- the server will cache the server IP for some time to reduce the time it takes to reach the server
- you can also make an entry into the host file and use search and the domain name. 
 there are A name records that maps a name to an ipv4 address
- You can use nslookup OR dig to query a hostname

nslookup www.google.com 
it only queries names from the DNS.

## Network Namespaces
    
- when you run the ps aux in a container deployed in a namespace, it lists the PID of the container as isolated with PID=1 
- when u run the ps aux on the host, it lists several processes running in the system including the container process.
- You can create a container with a network NameSpace that shields the container from the network-related information of the host.

ip netns add red #to create a network namespace
ip netsn # to list the network ns
ip link  # to like the interfaces
ip netns exec red ip link  # to view interfaces within the namespace
ip -n red addr add 196.168.15.1 dev veth-red # to assign an ip to a namespace 

- to connect the red and the blue namespaces, run the 
ip link veth-red type veth peer name veth-blue
- to attach the two interfaces, run the 
ip link set veth-red netns red  
- assign ip to the NameSpace
ip -n red addr add 196.168.15.1 dev veth-red
- bing up the interface using  
ip -n red lint set veth-red up  
- test connectivity using the 
ip netns exec red ping 192.168.15.2
- list the arp table on the red namespace
ip netns exec red arp  

To enable connectivity between multiple NameSpaces, you need a switch on the host server and
Use the Bridge network 
- Add a new interface and set it to bridge 
ip link add v-net-0 type bridge 
ip link # it will be status down so you need to bring it up
ip link set dev n-net-0 up
- to connect the NameSpaces to the network,  
ip link add veth-red type veth peer name veth-red-br
ip link add veth-blue type veth peer name  veth-blue-br

ip link set veth-red netns red 
ip link set veth-red-br master v-net-0
- assign ip addresses
 ip -n red addr add 192.168.15.2 dev veth-red
 ip -n red link set veth-red up

- Connection will not be established between the host and the NameSpace.
- to enable communication,  add the bridge network hosting the namespace to the host network
ip addr add 192.168.12.5/24 dev v-net-0

- These namespaces are not accessible over the internet
- A gateway needs to be added to the bridge network that allows connectivity. Since the local host has a gateway,
   we can add a route entry in the namespace to route traffic to the host ip.

## DOCKER NETWORK:
    
- When you create a docker container, you can specify a network for the container to run on.
docker run nginx --image=mginx --network none
- this container will not be accessible within or outside.
- with the host network, the container will be accessible by the host.
docker run --network host nginx.
if a port say 80 is open on the host, no other container can use that port.
- the third network is the bridge which has a default ip of 172.17.0.0
- when docker is installed, the bridge is created by default
docker network ls # ip link   # ip addr   # ip netns
docker creates an interface that attaches the container to the bridge network.
- the container is also assigned an IP  

## Port Mapping:

  - Since the container is on a private network, it can only communicate to another container on the host and not be accessed externally.
  - But the host has an internet gateway that allows traffic to the internet.
  - Therefore, a port is opened on the host that allows traffic from the container to pass through the host.

  docker run -p 8080:80 nginx

  port 8080 is opened on the host, while port 80 is opened on the container
  - To access the container externally 
  curl http://192.168.1.10:8080
- Docker does this by creating a nat rule on the ip table.

iptables \
       -t nat \
       -A DOCKER \
       -j DNAT \
       --dport 8080 \
       --to-destination 172.17.0.3:80 
to list the rules, run  
iptables etcd-versionl -  t nat 


## CONTAINER NETWORKING INTERFACE:
   

bridge add sf565f6s+6f /var/run/netns/sf565f6s+6f

this is an easier way to add a container to a container through a set standard of how containers should communicate.
CNI defines a plugin for how containers have to communicate.

## CLUSTER NETWORKING:
  
- each node must have at least one interface connected to a network.
- Each network must have an address configured.
- the host must have a unique hostname and a unique mac address.
- some ports must be opened.
6443 : Kube-api
10250 : kubelet  # both master and worker nodes
10251 : kube-scheduler
10252 : kube-controller-manager
2379 : ETCD
30000-32767 : Services # on the worker nodes

ip address
ip address shows the type of bridge
ip route # lists all routes to the internet
netstat --help
netstat -npl | grep -i scheduler # to see the port the scheduler listens on| same for any k8s component

to see the number of established Connections,
netstat -npa | grep -i etcd | grep -i 2379 | we -1

## POD NETWORKING:
  

- There is a network at the level of the nodes and also a network at the level of pods on the nodes to establish communication.
- It is not provided by default in k8s.
- K8s requires that every pod should have its IP addresses
- Every pod should be able to communicate with other pods on the same and other nodes without NAT.
- As long as this solution is implemented, the network will be established.
- there are many networking solutions such as WEAVE / FLANNERL / VMWARE NSX 
- https://www.weave.works/blog/weave-cloud-end-of-service
 https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
 Reference links: –

https://www.weave.works/docs/net/latest/kubernetes/kube-addon/#-installation
https://github.com/weaveworks/weave/releases

ls /etc/cni/net.d/  # to see the cni plugin used in the cluster

IPAM: Ip address management
the CNI (container network interface) is responsible for assigning IP addresses to pods in the k8s cluster

kubectl logs -n kube-system weave-net-mrks # to see the IP set to the pod 
kubectl exec <podname> -- ip route # to see the default route configured on the pod

## SERVICE NETWORKING:
   
- Pods are rarely configured to communicate with each other directory. they make use of k8s services.
- When a service is created, it is accessible to all pods on the cluster. it is called ClusterIP.
- It is used for pods intended to only be accessed within the cluster.
- if the pod was intended to be accessed externally, a NodePort service will be used, it routes traffic to the internet
  through a port on the host node. it is also accessible within the cluster.
- the kubelet service on the worker nodes which is responsible for creating pods also monitors the changes in the cluster 
  kube-api server. Each time a new pod is to be created, the kubelet involves the CNI plugin to configure networking for the pod.
- the kube-proxy watched changes through the kube-apiserver, and every time a new service is created the kube-proxy is activated.
- Services are cluster-wide resources. A service is assigned an IP address from a predefined range which is used  
  by the kube-proxy. 
- the kube-proxy used the service IP address and created a forwarding rule in the cluster, any traffic coming to the IP of the  
  service is forwarded to the IP of the pod. 
```sh
k get pods -o wide 
k get svc 
cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep cluster-ip-range  # to see the IP range for services in the cluster
iptables -L -t nat | grep <ServiceName>  # to see the routing rules
k logs -n kube-system <podname> # to see the proxy configured on the pod
```

## DNS RESOLUTION IN K8S:

- k8s objects within a cluster can refer to each other by calling the ip addresses of the objects within the same namespace.
  If an object is to communicate with another object on the dev namespace, the name of the object will be appended by  
  the name of the NameSpace.
  curl http://websetvice.dev # the last name of the service is the name of the namespace
- for each namespace, the dns sercer creates a subdomain for the cluster call dev.
- for all services in a cluster, the dns server creates another subdomain call svc 
- All services and pods are grouped for a root domain called local 

curl http://websetvice.dev.svc.cluster.local # fully qualified domain name for the service
+ the same is done for pods, except that they are not given names but rather the . in the ip are changed to - 
+ Kubernetes implements a DNS server in the cluster called COREDNS,
+ they are deployed as pods. they run the coreDNS executable.

## KUBERNETES INGRESS:
   
+ Scenario:
  - You deploy a web app as a pod to host an online store called a store.
  - A database is also deployed to write data from the store app.
  - To make a database accessible to the web app, a ClusterIP service is destroyed for the db.
  - To make the webapp accessible to external users, a NodePort service is deployed say with port 38080
  - to access the app on the internet, http://<nodeIP>:38080
- Whenever traffic increases, we increase the number of pods and traffic is routed by the service
- With a pdt grade app, we do not want users to type the IP every time, so u configure a DNS to access using a name.
- The name will be something like http://my-store:38080.
- Since you don't want users to remember the nodePort either, you bring an additional layer btw dns and service and point the DNS  
  to the server.
- In the case of a cloud platform, create a service of type LoadBalancer. the cloud will deploy a LoadBalancer.
- the LB has an external IP that can be shared with users to access the app.

As the company grows you want to add a video app to the store but as a separate app on the same cluster.
- you create a service for the new app and assign a LoadBalancer. 
- To redirect traffic to a specific, you need to configure another proxy on the two existing LoadBalancers.
- each time a user types a specific URL, the proxy routes the traffic to the desired app.
- You also need to enable SSL for the app so that the user can access the app using https.
- it can be done at the app level or service level. It is a complex setup.
- It can all be managed on the k8s cluster just like an object.

- Ingress permits you to provide a single endpoint to access multiple applications as well as manage traffic routing between
  the defined URL path. It also configures ssl for http access. It is a layer 7 LoadBalancer in k8s.
- You still need to expose ingress either using NodePort or Cloud Native LoadBalancer.

- First, deploy a reverse proxy like nginx/haproxy   # INGRESS CONTROLLER
- specify a set of rules to specify ingress   # INGRESS RESOURCES

## Ingress Controller:
   
They are created using a definition file. the cluster doesn't come with an ingress  controller.
- To deploy ingress, use any of  GCP HTTP LB  OR NGINX   # Supported and maintained by K8S
- They are not just LB, they have additional features 
- it is deployed using  

nginx-ingress.yaml
```sh
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nginx-ingress-controller
spec:
  replicas: 1
  selector:
    matchLabels:
      name: nginx-ingress
  template:
    metadata:
      labels:
        name: nginx-ingress
    spec:
      containers:
        - name: nginx-ingress-controller
          image: quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.21.0
          args: 
            - /nginx-ingress-controller
            - --configmap=(POD_NAMESPACE) /nginx-configuration
          env:
            - name: POD_NAME
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name 
            - name: POD_NAMESPACE 
              valueFrom:
                fieldRef: 
                  fieldPath: metadata.namespace
          ports:
            - name: http
              containerPort: 80
            - name: https
              containerPort: 443
```
---
nginx-configmap.yaml
```sh
kind: ConfigMap
apiVersion: v1
metadata:
  name: nginx-configuration
```
---
nginx-ingress-svc.yaml
```sh
apiVersion: v1
kind: Service
metadata:
  name: nginx-ingress
spec:
  type: NodePort
  ports:
    - port:
      targetPort: 80
      protocol: TCP
      name: http 
    - port:
      targetPort: 443
      protocol: TCP
      name: https
    selector: nginx-ingress
```
---
nginx-service-account.yaml
```sh
kind: ServiceAccount
apiVersion: v1
metadata:
  name: nginx-ingress-serviceaccount
```

- the nginx program is stored within the image as /nginx-ingress-controller. it must be passed to start the nginx service
- to decouple this data from the nginx object, create a configmap object with no entries and pass it in
- Pass two env that carry the pod name and namespace to enable the nginx server to read the configuration data from the pod.
- specify the pods used by the ingress controller
- Create a service for the ingress controller
- Create a service account with roles and rolebinding for permissions for the ingress account to access and 
   manage the different app 

## Ingress Resources:
   
- An ingress resource is a set of rules applied to the ingress controller.
- Here you can specify that if a user inputs a name, route them to the store etc.

ingress-wear.yaml
```sh
apiVersion: extensions/v1beta1
kind: Ingress 
metadata: nginx-wear
spec:
  backend:
    serviceName: wear-service   # the name of the service for the app
    servicePort: 80    # the service port.
```
k create ingress <IngressName> - <Namespace> --rule="/watch=ingress-service:8080 --rule"/wear=ingress-service:8080 "


k create -f <filename>
k get ingress  

- Rules are used when you want to route traffic based on different conditions, traafic originating for diff subdomains
- You can specify multiple rules depending on the number of subdomains existing on the app.

rule 1: www.my-online-store.com /waer   / watch 
rule2: www.wear.my-online-store.com     /returns  /support
rule3: www.watch.my-online-store.com 
rule4: anything els /movies  /tv

ingress resources for multiple rules:

  ingress-resource-rules.yaml
```sh
apiVersion: extensions/v1beta1
kind: Ingress 
metadata: nginx-wear
spec:
  rules:
  - http:
    paths:
    - path: /wear  
      backend:
        serviceName: wear-service
        servicePort: 80
    - path: /watch
      backend:
        serviceName: watch-service
        servicePort: 80
```
kubectl create ingress ingress-wear -n <Namespace> --rule="/wear=wear-service:8080"

k describe ingress ingress-resource-rules

- To route traffic based on domain names, add the hsot filed in the spec.
```sh
apiVersion: extensions/v1beta1
kind: Ingress 
metadata: nginx-wear
spec:
  rules:
  - host: wear.my-online-store.com
    http:
      paths:  
      - backend:
          serviceName: wear-service
          servicePort: 80
  - host: watch.my-online-store.com
    http:
      paths:  
      - backend:
          serviceName: watch-service
          servicePort: 80
```
    
Find more information and examples in the below reference link:-

https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-ingress-em- 

References:-

https://kubernetes.io/docs/concepts/services-networking/ingress

https://kubernetes.io/docs/concepts/services-networking/ingress/#path-types    

## Multi Container PODS:

the idea of decoupling a large mom=nolithic application into small components called microservices,
allows us to deploy a set of small independent and reusable code. This setup allows us to manage, or update only,
small portions of the app instead of the entire app. It might require running two apps or components in the same  
container. An example is a web server and a log agent deployed in the same container. They share the same lifecycle,
they are created and destroyed together, and they share the same network and the same volume of resources.
```sh
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    name: webapp
spec:
  containers:
  - image: nginx     # container1
    name: nginx
    ports:
      - containerPort: 8080
  - image: log-agent     #container2
    name: log-agent
```
https://kubernetes.io/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume/
    
to exec into a container in kubernetes, run the 
```sh
kubectl -n <Namespace> exec -it <ContainerName> -- cat /log/app.log
```
### InitContainers:

  these are also sidecar containers just like in a multicontainer pod. But they do not run constantly like the multi-containers
  Init containers are designed to run a particular process and then once the process is complete, they are exited.
  they may provide a service or run a script that starts a process in the main container and then exits.
```sh
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox
    command: ['sh', '-c', 'git clone <some-repository-that-will-be-used-by-application> ;']
```
# LOGGING AND MONITORING:

We can monitor the applications deployed in k8s as well as the Kubernetes cluster.
to monitor resources in the cluster, we can monitor
- node level metrics  #number of nodes, healthy, memory performance, CPU utilization
- pod level metric # number of pods and their CPU utilization

Kubernetes by default does not come with any monitoring agent but metric servers can be used and other resources  
such as Prometheus.
- You can have one metric server per cluster, the metric server retrieves metrics from pods and nodes  
 and stores them in memory. It does not store the metric in the disk so you can not store see historical metric.

You can clone the metric server from the github repo and run it. 
cluster performance can be seen by running  

kubectl top node 
kubectl top pods

managing application logs: 
  When you run a container in a pod, you can stream the logs by running the  
   kubectl logs -f <podname>
in the case of multi-container pods, you need to specify the name of the container individually.

# CLUSTER MAINTENANCE:

This is important to know how and when to upgrade a cluster, and how to understand disaster recovery.
### 1. *OS UPGRADE:*
  To take down nodes in the cluster for updates or security patches on the node. When a node hosting the pods goes down,
  all the pods will not be accessible to users. Except there were replicas of that pod on another node.
  If the node lasts less than 5 minutes, the pods can be rescheduled, but if it exceeds 5 minutes, the controller will  
  consider it dead. If the pods were part of a ReplicaSet, they are recreated on other nodes depending on the nodeAffinity policies

  A quick update can be done when you are sure it will last less than 5 minutes, and if the pods on that node are part of a ReplicaSet.
  this will ensure that the application remains accessible.
  To safely do an upgrade on the nodes, you can drain the node for
```sh
  kubectl drain node-1 # --ignore-daemonsets 
  kubectl cordon node-2  # to make node unschedulable
  kubectl uncordon node-2 
```
  The node will be marked as unschedulable and pods will be gracefully terminated and recreated on other nodes.
  You can, path the nodes and make them available. You need to manually uncordon the node to make it schedulable.

  After a node is uncordon, it will require that pods are scheduled on it afresh, pods that were evicted during the drain 
  will not be automatically rescheduled.

  If a pod is not part of a ReplicaSet on a node, k8s internal security will not permit that node to be drained unless
  you manually delete the pod.
  nevertheless, you can use  --force flag to force delete the pod. This will permanently delete the pod on that node  
  and will not recreate it on another node because it was not part of a ReplicaSet.

### 2. *Cluster Upgrade:*
  Kubernetes is released in version and there are minor versions such as the alpha and beta versions before a more stable 
  release is made.
  None of the cluster components can be of a version higher than the API Server, except for the kubelet service.
  You can upgrade component by component.
  At any time, k8s supports only the latest three minor versions. It is good to upgrade your cluster before the version is unsupported.
  You should upgrade only one version higher at a time and not to the latest version if you were not using the previous one.

the upgrade depends on how the cluster is set up. between managed and self-managed clusters. managed clusters provided by the cloud is easier.
the cluster is being upgraded component by component and while the master node is being upgraded, the Kube API, and controllers,  
go down briefly. this does not affect the worker nodes.

To upgrade the worker nodes, if you do all of them at once, users will not be able to access the app. 
You can also upgrade one node at a time, which guarantees your pods are not all down. Or u use new nodes with newer
software and then move all pods?


https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/
```sh
cat /etc/*release*  # to see the OS the nodes are using
kubeadm upgrade plan # to all the latest and stable versions
k drain controlplane --ignore-daemonsets
apt update
apt-cache madison kubeadm  # select the kubeadm version
apt-get upgrade -y kubeadm=1.12.0-00  # It has to be upgraded before the cluster components
```

to upgrade the cluster, use the 
```sh
kubeadm upgrade apply v1.12.0
```
the next step is to upgrade the kubelet.
the kubelet service must be upgraded manually
```sh
apt-get upgrade kubelete=v1.12.0-00
systemctl restart kubelet.service
```
### 3. *Node Upgrade:*

  https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/upgrading-linux-nodes/

  You need to first move all the workloads from the node to other nodes using the 
  ```sh
  kubectl drain node01 # This makes the node unschedulable
  apt-get upgrade -y kubeadm=v1.12.0-00
  apt-get upgrade -y kubelete=1.12.0-00
  kubeadm upgrade node config --kubelet-version v1.12.0
  systemctl restart kubelet
  kubectl uncordon node01
```

  perform the same steps to upgrade all other worker nodes.

  ssh <nodename>

# BACKUP AND RESTORE:

Concerning resources, declarative approaches can be used to save your configuration files. A good practice is to save
these codes in a source code repo like GitHub. But if an object is created imperatively, it will be difficult to keep track.
therefore, the KubeAPI server is a place to get all created resources.
All resource configurations are saved in the kube-apiserver
```sh
 kubectl get all --all-namespaces -o yaml > all-deploy.yaml
```
+ There are tools like VELERO that can help in taking backups of the cluster.

The ETCD stores information about the state of the cluster.
So you can choose to backup the etcd cluster itself. it is found in the control-plane
data is stored in the data directory.
it also comes with the built-in snapshot utility
```sh
etcdctl snapshot save snapshot.db
etcdctl snapshot status snapshot.db
```

+ A snapshot directory is created. To restore the cluster from this backup, stop the kubeapi server and run the  
```sh
etcdctl snapshot restore snapshot.db --data-dir /var/lib/etcd-from-backup
systemctl daemon-reload
service kube-apiserver start
```
```sh
k logs etcd-controlplane -n kube-system | grep -i 'etcd-version'
```

ls /etc/kubernetes/manifest
k edit etcd.yaml
export ETCDCTL_API=3
```sh
ETCDCTL_API=3 etcdctl snapshot save --endpoint= \
--cacert= \
--cert= \
--key= \
/opt/snapshot-pre-boot.db  #location to save backup
```
+ To restore the original state of the cluster using the backup file, you can use the etcd restore <filename>

```sh
etcdctl snapshot restore --data-dir /var/lib/etcd-from-backup /opt/snapshot-pre-boot.db 
```
ls /var/lib/etcd-from-backup

vi /etc/kubernetes/manifest/etcd.yaml

edit the Hostpath and add /var/etcd-from-backup

*NOTE*:

https://www.hostinger.com/tutorials/kubernetes-tutorial?utm_campaign=Generic-Tutorials-DSA|NT:Se|LO:Other-EU&utm_medium=ppc&gad_source=1&gclid=Cj0KCQjw4MSzBhC8ARIsAPFOuyVhED0cRJ2C0zKzYeGg4fjnBEnixImC46C2Mn5efFZ5INqNKIBVfEoaAvHxEALw_wcB
