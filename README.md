it  **Author: Prince Chafah Forchu Sani**
  
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
# Deploying a StatefulSet on AWS EKS with EBS CSI Driver and IRSA

This guide walks you through the deployment of a Kubernetes `StatefulSet` on AWS EKS. It uses:

- AWS EBS volumes provisioned dynamically via the EBS CSI Driver
- Kubernetes-native persistent volume claims (PVC)
- An optional IRSA setup using IAM Roles for Service Accounts (STS) — though no AWS services are accessed directly
- A custom Docker image: `hilltopconsultancy/globe:v1`

---

## What Is a StatefulSet?

A `StatefulSet` manages stateful applications in Kubernetes. Unlike `Deployments`, it provides:

- **Stable network identity** (e.g., `pod-0`, `pod-1`)
- **Stable storage** (each pod gets its own volume)
- **Ordered deployment and scaling**

Use cases:
- Databases (PostgreSQL, MongoDB)
- Caches (Redis, Memcached)
- Applications needing persistent local data

---

##  Prerequisites

- EKS cluster (`eks-orange-prod`) running in region `eu-central-1`
- `kubectl` and `eksctl` installed and configured
- Node group with EC2 instances in subnets tagged for ELB and CSI
- The following environment variables:

```bash
export CLUSTER_NAME=eks-orange-prod
export REGION=eu-central-1
export ACCOUNT_ID=<your-aws-account-id>
````

---

## Step 1: Associate OIDC Provider with the EKS Cluster

```bash
eksctl utils associate-iam-oidc-provider \
  --cluster $CLUSTER_NAME \
  --region $REGION \
  --approve
```

Verify:

```bash
aws eks describe-cluster \
  --name $CLUSTER_NAME \
  --region $REGION \
  --query "cluster.identity.oidc.issuer" \
  --output text
```

Save the ID at the end of the URL for later (e.g., `OIDC_ID=xxxxx`).

---

## Step 2: Set Up the EBS CSI Driver Role (IRSA)

### 2.1 Download the IAM policy for EBS CSI

```bash
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-ebs-csi-driver/master/docs/example-iam-policy.json
```

### 2.2 Create IAM policy

```bash
aws iam create-policy \
  --policy-name AmazonEBSCSIDriverPolicy \
  --policy-document file://example-iam-policy.json
```

### 2.3 Create IAM role for CSI driver

Replace `<OIDC_ID>` below with your actual OIDC ID:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::<ACCOUNT_ID>:oidc-provider/oidc.eks.<REGION>.amazonaws.com/id/<OIDC_ID>"
      },
      "Action": "sts:AssumeRoleWithWebIdentity",
      "Condition": {
        "StringEquals": {
          "oidc.eks.<REGION>.amazonaws.com/id/<OIDC_ID>:sub": "system:serviceaccount:kube-system:ebs-csi-controller-sa"
        }
      }
    }
  ]
}
```

Save as `ebs-trust-policy.json`, then run:

```bash
aws iam create-role \
  --role-name AmazonEBSCSIDriverRole \
  --assume-role-policy-document file://ebs-trust-policy.json

aws iam attach-role-policy \
  --role-name AmazonEBSCSIDriverRole \
  --policy-arn arn:aws:iam::$ACCOUNT_ID:policy/AmazonEBSCSIDriverPolicy
```

### 2.4 Annotate the service account for the CSI driver

```bash
kubectl annotate serviceaccount ebs-csi-controller-sa \
  -n kube-system \
  eks.amazonaws.com/role-arn=arn:aws:iam::$ACCOUNT_ID:role/AmazonEBSCSIDriverRole
```

---

## Step 3: Create a Kubernetes ServiceAccount

We create a simple ServiceAccount for your StatefulSet (no AWS permissions required):

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-app-sa
  namespace: default
```

Save as `serviceaccount.yaml` and apply:

```bash
kubectl apply -f serviceaccount.yaml
```

---

## Step 4: Create the gp3 StorageClass

Create a new `gp3` storage class for EBS:

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: gp3
provisioner: ebs.csi.aws.com
parameters:
  type: gp3
  fsType: ext4
reclaimPolicy: Delete
volumeBindingMode: WaitForFirstConsumer
```

Save as `gp3-storageclass.yaml` and apply:

```bash
kubectl apply -f gp3-storageclass.yaml
```

---

## Step 5: Create a Headless Service for StatefulSet

```yaml
apiVersion: v1
kind: Service
metadata:
  name: globe
spec:
  clusterIP: None
  selector:
    app: globe
  ports:
    - port: 80
```

Save as `headless-svc.yaml` and apply:

```bash
kubectl apply -f headless-svc.yaml
```

---

##  Step 6: Deploy the StatefulSet

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  serviceName: "globe"
  replicas: 1
  selector:
    matchLabels:
      app: globe
  template:
    metadata:
      labels:
        app: globe
    spec:
      serviceAccountName: my-app-sa
      containers:
        - name: globe
          image: hilltopconsultancy/globe:v1
          ports:
            - containerPort: 80
          volumeMounts:
            - name: www
              mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
    - metadata:
        name: www
      spec:
        accessModes: ["ReadWriteOnce"]
        storageClassName: gp3
        resources:
          requests:
            storage: 1Gi
```

Save as `statefulset.yaml` and apply:

```bash
kubectl apply -f statefulset.yaml
```

---

##  Step 7: Verify Deployment

Check status:

```bash
kubectl get pods
kubectl get pvc
kubectl describe pod web-0
```

Expected:

* Pod `web-0` is running
* PVC `www-web-0` is `Bound`
* EBS volume created and attached automatically

---

##  **2. DaemonSet**

###  **Definition:**

A **DaemonSet** ensures that a pod runs on **every node** (or a selected group of nodes) in the cluster. It's used for **node-level services** like monitoring agents or log shippers.

Every time a new node joins the cluster, the DaemonSet automatically adds the pod to it.

---

###  **Use Case:**

Used for **background services** that must run on all nodes, such as:

* Monitoring (e.g., Prometheus Node Exporter)
* Log collection (e.g., Fluentd, Filebeat)
* Security tools (e.g., Falco)
* CSI storage drivers (e.g., AWS EBS CSI)

---

###  **DaemonSet Example Using Prometheus Node Exporter**

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: node-exporter
  labels:
    app: node-exporter
spec:
  selector:
    matchLabels:
      app: node-exporter
  template:
    metadata:
      labels:
        app: node-exporter
    spec:
      containers:
        - name: node-exporter
          image: prom/node-exporter
          ports:
            - containerPort: 9100
              name: metrics
```
---

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
  --region <Region> \
  --cluster <ClusterName>\
  --approve
```

### 4. Create IAM Service Account
Create an IAM service account for the controller:
```bash
eksctl create iamserviceaccount \
  --region <Region> \
  --cluster <ClusterName> \
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
  --set clusterName=<ClusterName> \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller \
  --set region=<Region> \
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
        image: hilltopconsultancy/globe:v1
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
  --from-literal=COLOR=green \
  --from-literal=COUNTRY=Cameroon \
  --from-literal=LOG_LEVEL=debug \
  --from-literal=CALENDAR=false
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
  CALENDER: true
  LOG_LEVEL: debug
  COUNTRY: Cameroon
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
        image: hilltopconsultancy/globe:v1
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
        image: hilltopconsultancy/globe:v1
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
        image: hilltopconsultancy/globe:v1
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

Kubernetes `Secrets` let you store **sensitive information** such as:

- Database passwords
- API tokens
- SSH keys

Unlike ConfigMaps, Secrets are **base64-encoded** and intended to keep sensitive data separate from your application code.

---

##  Use Cases

You should use Secrets when:
- You want to **inject credentials** or API keys into your containers.
- You don’t want to hardcode sensitive values into container images or YAML manifests.
- You want better control over **secret management and auditing**.

---
##  How to Create a Secret

###  Option A: Imperatively from CLI

```bash
kubectl create secret generic app-secret \
  --from-literal=DB_HOST=mysql \
  --from-literal=DB_USER=root \
  --from-literal=DB_PASSWORD=secret123
````

###  Option B: Declaratively from a YAML file

First, encode values with `base64`:

```bash
echo -n 'mysql' | base64        # Output: bXlzcWw=
echo -n 'root' | base64         # Output: cm9vdA==
echo -n 'secret123' | base64    # Output: c2VjcmV0MTIz
```

Then create `secret.yaml`:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
data:
  DB_HOST: bXlzcWw=
  DB_USER: cm9vdA==
  DB_PASSWORD: c2VjcmV0MTIz
```

Apply it:

```bash
kubectl apply -f secret.yaml
```

---

##  How to View Secrets

```bash
kubectl get secret app-secret
kubectl describe secret app-secret
kubectl get secret app-secret -o yaml
```

To decode values:

```bash
echo -n 'c2VjcmV0MTIz' | base64 --decode
```

---

## Deploy a Test Application that Reads Secrets

We’ll deploy a simple app that prints secret values to logs on startup.

###  `pod-with-secret.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-demo
spec:
  serviceAccountName: default
  containers:
    - name: demo
      image: busybox
      command: [ "/bin/sh", "-c" ]
      args:
        - echo "Connecting to DB with $DB_USER@$DB_HOST"; echo "Password: $DB_PASSWORD"; sleep 3600
      envFrom:
        - secretRef:
            name: app-secret
```

Deploy it:

```bash
kubectl apply -f pod-with-secret.yaml
```

---

## Test If the Secret Was Injected

Check the pod's logs:

```bash
kubectl logs secret-demo
```

Expected output:

```
Connecting to DB with root@mysql
Password: secret123
```
---

## ❗ Security Notes

* Kubernetes Secrets are **not encrypted at rest by default** — they are only base64-encoded.
* Use Kubernetes encryption providers (e.g., KMS, Vault) to enable encryption at rest.
* **Do NOT commit Secrets to GitHub.**

---

## Persistent Volume (PV) and Persistent Volume Claim (PVC)

### 1. Persistent Volume (PV)

**Definition**:
A `PersistentVolume` is a piece of storage in the cluster provisioned by an admin or dynamically created by Kubernetes using a storage class (e.g., AWS EBS). It is a cluster resource.

**Use Case**:
Used when you want to manage storage independently from pods, allowing data persistence across pod restarts or rescheduling.

**Note**:
When using dynamic provisioning (like with EBS `gp3`), you don't need to create a PV manually — Kubernetes creates it automatically when you create a PVC.

---

### 2. Persistent Volume Claim (PVC)

**Definition**:
A `PersistentVolumeClaim` is a request for storage by a user. It specifies size, access mode, and storage class.

**Use Case**:
Used by pods to request storage without knowing the details of the underlying volume. It acts like an "interface" to access the PV.

---

### 3. Access Modes

**Definition**:
Access modes define how a volume can be mounted by pods.

| Access Mode   | Description                            | Supported by AWS EBS |
| ------------- | -------------------------------------- | -------------------- |
| ReadWriteOnce | Mounted as read-write by a single node | Yes                  |
| ReadOnlyMany  | Mounted as read-only by many nodes     | No                   |
| ReadWriteMany | Mounted as read-write by many nodes    | No                   |

**Explanation**:
AWS EBS only supports **ReadWriteOnce**, which means:

* Only one node can mount the volume at a time.
* You can scale pods across replicas only if each pod mounts a different volume.

---

### 4. Example: PVC and Deployment Using AWS EBS (`gp3`)

#### Step 1: Create a PVC

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: ebs-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: gp3
```

This tells Kubernetes to dynamically create a 5Gi EBS volume using the `gp3` storage class and bind it to this claim.

---

#### Step 2: Create a Deployment that Mounts the PVC

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-ebs-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-ebs
  template:
    metadata:
      labels:
        app: nginx-ebs
    spec:
      containers:
      - name: nginx
        image: nginx
        volumeMounts:
        - name: ebs-storage
          mountPath: /usr/share/nginx/html
      volumes:
      - name: ebs-storage
        persistentVolumeClaim:
          claimName: ebs-pvc
```

**Explanation**:

* The deployment runs an `nginx` container.
* The container mounts the EBS volume at `/usr/share/nginx/html`.
* If the pod is deleted and recreated, the data in the volume remains.

---

### 5. Summary

* Use **PVCs** to request storage in a standardized way.
* Kubernetes automatically provisions an **EBS volume** via dynamic provisioning using `gp3`.
* **Access modes** define how the volume can be shared.
* AWS EBS supports only **ReadWriteOnce** — one node at a time can access the volume in read/write mode.
* This approach is ideal for **stateful applications** like databases and CMSs.

---

## Volume Lifecycle:
The reclaim policy for a PersistentVolume tells the cluster what to do with the volume after it has been released of its claim
+ Recycle
+ Retain
+ Delete


### NAMESPACES:


# Kubernetes Namespaces: Definition, Use Case, and Cross-Namespace Demo

---

##  What Are Kubernetes Namespaces?

A **namespace** in Kubernetes is a virtual cluster within a physical cluster. Namespaces allow you to organize and manage resources in logically isolated groups.

By default, Kubernetes comes with the `default`, `kube-system`, `kube-public`, and `kube-node-lease` namespaces.

---

## Why Use Namespaces?

Namespaces are useful when you want to:

| Use Case                     | Benefit                                                        |
|-----------------------------|----------------------------------------------------------------|
| **Environment separation**   | Keep `dev`, `staging`, and `prod` resources logically isolated |
| **Team isolation**           | Give different teams (`frontend`, `backend`) their own space   |
| **Security boundaries**      | Apply RBAC rules specific to users per namespace               |
| **Resource quotas**          | Prevent noisy-neighbor problems by limiting per-namespace usage|
| **Multi-tenancy**            | Run workloads from different clients or applications securely  |

---
kubectl apply -f <filename>  ==> will create object in the default namespace
kubectl create -f <filename> --namespace=kubesystem  ==> will create object in the kubesystem namespace

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
## Demo: Create and Use Two Namespaces (`finance` & `devops`)

We will:
- Create two namespaces
- Deploy an app in each namespace
- Verify isolation
- Demonstrate **cross-namespace access** using DNS

---

##  Step 1: Create the Namespaces

```bash
kubectl create namespace finance
kubectl create namespace devops
````
---

##  Step 2: Deploy an App in Each Namespace

###  `finance-pod.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: finance-api
  namespace: finance
  labels:
    app: finance-api
spec:
  containers:
    - name: app
      image: hashicorp/http-echo
      args:
        - "-text=Hello from Finance"
      ports:
        - containerPort: 5678
---
apiVersion: v1
kind: Service
metadata:
  name: finance-service
  namespace: finance
spec:
  selector:
    app: finance-api
  ports:
    - port: 80
      targetPort: 5678
```

```bash
kubectl apply -f finance-pod.yaml
```

---

###  `devops-pod.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: devops-api
  namespace: devops
  labels:
    app: devops-api
spec:
  containers:
    - name: app
      image: hashicorp/http-echo
      args:
        - "-text=Hello from DevOps"
      ports:
        - containerPort: 5678
---
apiVersion: v1
kind: Service
metadata:
  name: devops-service
  namespace: devops
spec:
  selector:
    app: devops-api
  ports:
    - port: 80
      targetPort: 5678
```

```bash
kubectl apply -f devops-pod.yaml
```

---

## Step 3: Verify Isolation

```bash
kubectl get pods -n finance
kubectl get pods -n devops
```

Try to reference a `Secret`, `Pod`, or `Service` from another namespace — it will **not work unless fully qualified**.

---

##  Step 4: Access a Service Across Namespaces (Using DNS)

### Exec into the devops pod:

```bash
kubectl exec -it devops-api -n devops -- sh
```

### Inside the container:

```sh
wget -qO- http://finance-service.finance.svc.cluster.local
```

 You’ll get: `Hello from Finance`

---

## Step 5: Attempt Invalid Cross-Namespace Access

Try accessing a ConfigMap or Secret from `finance` inside `devops`:

```bash
kubectl get secrets -n finance
kubectl get secrets -n devops
```

You’ll notice:

* Secrets/ConfigMaps are **namespace-scoped**
* They must be duplicated across namespaces if needed

---

##  DNS Format for Cross-Namespace Services

```
http://<service-name>.<namespace>.svc.cluster.local
```

---

##  Best Practices and Logical Uses

| Namespace Design Pattern | Description                                       |
| ------------------------ | ------------------------------------------------- |
| `team-env`               | e.g., `frontend-dev`, `backend-prod`              |
| `client-name`            | e.g., `tenant-a`, `tenant-b` for multi-tenancy    |
| `feature-branch`         | e.g., `feature-login-refactor` for temporary test |

### Recommendations:

* Use namespaces in **RBAC** to scope permissions.
* Use **ResourceQuotas** to cap CPU/memory usage per team.
* Avoid clutter: don’t use namespaces as folders for every small app — use them meaningfully.

---

##  Summary

Namespaces are critical for:

* Organizing workloads
* Providing logical separation
* Securing workloads with RBAC
* Supporting complex workflows and multi-user environments

---


### Resource Quota:

A `ResourceQuota` is used to **limit resource consumption** per namespace. It helps control how many resources (pods, CPU, memory, etc.) a team or project can use in a shared cluster.

###  **Common ResourceQuota Types**

#### 1. **Limit by Pod Count Only**

Limits how many pods can be created in the namespace.

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: pod-limit
  namespace: dev
spec:
  hard:
    pods: "10"
```

---

####  2. **Limit by Memory and CPU Requests Only**

Controls guaranteed minimum resource usage.

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-requests
  namespace: dev
spec:
  hard:
    requests.cpu: "2"
    requests.memory: 4Gi
```

---

####  3. **Limit by CPU and Memory Limits Only**

Controls the maximum resources pods are allowed to consume.

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-limits
  namespace: dev
spec:
  hard:
    limits.cpu: "4"
    limits.memory: 6Gi
```

---

#### 4. **Combined ResourceQuota: Pods + CPU + Memory**

Controls pod count and both requested and maximum resource usage.

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: full-quota
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: "2"
    requests.memory: 3Gi
    limits.cpu: "4"
    limits.memory: 5Gi
```

###  Notes:

* `requests` = guaranteed minimum the pod needs.
* `limits` = maximum the pod is allowed to use.
* Applies **per namespace**.
* Helps avoid resource starvation in multi-team clusters.

---

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
---

## Node Affinity in Kubernetes (EKS)

**Node Affinity** lets you control **which nodes your pod can be scheduled on**, based on node labels. It’s preferred over `nodeSelector` because it's more flexible and expressive.

###  Types of Node Affinity

1. **requiredDuringSchedulingIgnoredDuringExecution**

   * **Hard rule**: Pod *must* be scheduled on a node that matches.
2. **preferredDuringSchedulingIgnoredDuringExecution**

   * **Soft rule**: Scheduler will try to match, but can ignore it if no suitable node exists.

---

### Example: Schedule pod on nodes in `us-east-1a`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: topology.kubernetes.io/zone
            operator: In
            values:
            - us-east-1a
```

 In EKS, nodes are **automatically labeled** with their zone using:

```bash
topology.kubernetes.io/zone=<az>
```

---

##  Node Anti-Affinity

**Node Anti-Affinity** prevents pods from being scheduled on the **same node** with other pods matching specific labels.

---

###  Example: Avoid placing pod on a node with pods labeled `app=nginx`

```yaml
affinity:
  podAntiAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
    - labelSelector:
        matchExpressions:
        - key: app
          operator: In
          values:
          - nginx
      topologyKey: "kubernetes.io/hostname"
```

 `topologyKey: kubernetes.io/hostname` ensures the pod avoids the **same node**.
You can also use `topology.kubernetes.io/zone` to spread pods across **AZs**.

---

##  Summary

| Type               | Purpose                           | Key Fields                                    |
| ------------------ | --------------------------------- | --------------------------------------------- |
| Node Affinity      | Schedule pods on specific nodes   | `nodeAffinity` → `required` or `preferred`    |
| Node Anti-Affinity | Avoid certain pods sharing a node | `podAntiAffinity` → `required` or `preferred` |


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
Here are concise notes and examples on **Kubernetes Quality of Service (QoS)** classes:

---
###  **Kubernetes QoS Classes Overview**

Kubernetes uses **QoS classes** to manage pod scheduling and resource allocation. QoS is determined based on how you set **CPU/memory resource requests and limits** in the Pod spec.

---

### 1. **BestEffort**

* **Description**:

  * No CPU or memory *requests* or *limits* set.
  * Lowest priority; first to be evicted under resource pressure.
* **Use Case**: Non-critical, test, or batch workloads.

**Example:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: besteffort-pod
spec:
  containers:
    - name: busybox
      image: busybox
      command: ["sleep", "3600"]
```

---

### 2. **Burstable**

* **Description**:

  * CPU or memory *request* is set, but *limit* is not, or limit > request.
  * Can burst beyond the requested amount if resources are free.
  * Medium eviction priority.
* **Use Case**: Moderate importance workloads that can share resources.

** Example:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: burstable-pod
spec:
  containers:
    - name: nginx
      image: nginx
      resources:
        requests:
          memory: "128Mi"
          cpu: "250m"
        limits:
          memory: "256Mi"
          cpu: "500m"
```

---

### 3. **Guaranteed**

* **Description**:

  * *Both* CPU and memory requests **equal** limits.
  * Highest priority; least likely to be evicted.
* **Use Case**: Critical or production-grade workloads.

** Example:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: guaranteed-pod
spec:
  containers:
    - name: redis
      image: redis
      resources:
        requests:
          memory: "256Mi"
          cpu: "500m"
        limits:
          memory: "256Mi"
          cpu: "500m"
```

---

### Summary Table

| **QoS Class** | **Requests** | **Limits** | **Eviction Priority** |
| ------------- | ------------ | ---------- | --------------------- |
| BestEffort    | Not set      | Not set    | Lowest                |
| Burstable     | Set          | Optional   | Medium                |
| Guaranteed    | = Limits     | = Requests | Highest               |

---

- **BestEffort:** Pods with no resource requests or limits.
- **Burstable:** Pods with resource requests, but without memory limits or with memory limits lower than their requests.
- **Guaranteed:** Pods with both CPU and memory limits set to be higher than or equal to their resource requests.


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


---


This guide explains how to authenticate and authorize users to an Amazon EKS cluster using **IAM users** with **access keys**, the **aws-auth ConfigMap**, and **Kubernetes RBAC**.

---

## Overview

- **Authentication**: Handled by AWS IAM (using access keys).
- **Authorization**: Handled by EKS's `aws-auth` ConfigMap and Kubernetes RBAC.
- **Goal**: Grant a user read-only access to pods and services in the cluster.

---

##  Prerequisites

- An existing EKS cluster with `aws-auth` ConfigMap access enabled.
- AWS CLI and `kubectl` configured on your local machine.
- IAM permissions to edit the `aws-auth` ConfigMap and apply RBAC resources.

---

##  Step-by-Step Guide

---

### Step 1: Create an IAM User with Access Keys

1. Go to **IAM > Users > Add user**
2. User name: `dev-alice`
3. Select **Programmatic access**
4. Finish creation and **save the Access Key ID and Secret Access Key**

---

### Step 2: Attach IAM Policy for `eks:DescribeCluster`

This allows the IAM user to fetch cluster info when using `kubectl`.

#### Policy JSON:

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": "eks:DescribeCluster",
    "Resource": "*"
  }]
}
````

#### How to attach:

* Go to **IAM > Users > dev-alice > Permissions**
* Click **Add permissions**
* Choose **Attach policies directly**
* Click **Create inline policy**
* Paste the above JSON
* Name the policy `EKSDescribeClusterAccess` and attach it

---

### Step 3: Add IAM User to the `aws-auth` ConfigMap

On your admin machine:

```bash
kubectl edit configmap aws-auth -n kube-system
```

Add this under `mapUsers`:

```yaml
mapUsers:
  - userarn: arn:aws:iam::<account-id>:user/dev-alice
    username: dev-alice
    groups:
      - eks-viewers
```

> Replace `<account-id>` with your actual AWS account ID.

---

### Step 4: Create RBAC Role and Binding for `eks-viewers`

Save this to `viewer-role.yaml`:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: viewer-role
rules:
- apiGroups: [""]
  resources: ["pods", "services"]
  verbs: ["get", "list"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: bind-viewers
subjects:
- kind: Group
  name: eks-viewers
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: viewer-role
  apiGroup: rbac.authorization.k8s.io
```

Apply the RBAC config:

```bash
kubectl apply -f viewer-role.yaml
```

---

### Step 5: Configure AWS CLI as the IAM User

On Alice’s machine:

```bash
aws configure --profile alice
```

Enter:

* Access Key
* Secret Key
* Region (e.g., `eu-west-1`)
* Output format (e.g., `json`)

---

### Step 6: Connect to the Cluster

```bash
aws eks update-kubeconfig --name <your-cluster-name> --region <region> --profile alice
```

Test access:

```bash
kubectl get pods        # ✅ Should work
kubectl delete pod xyz  # ❌ Should be forbidden
```


---
# OPTION 2


1. `EKSAdmin`
2. `EKSDeveloper`
3. `EKSViewer`

# Amazon EKS Authentication & Authorization – Method 2: IAM Identity Center Access Portal

This guide explains how to authenticate and authorize users to an Amazon EKS cluster using **IAM Identity Center (Access Portal)**, **permission sets**, and **Kubernetes RBAC**.

---

## Overview

- **Authentication**: Handled by IAM Identity Center (via Access Portal and `aws sso login`)
- **Authorization**: IAM Identity Center Permission Sets + Kubernetes RBAC
- **Goal**: Grant users varying access to the EKS cluster via predefined permission sets:
  - `EKSAdmin`: Full cluster admin
  - `EKSDeveloper`: Read/write access to workloads
  - `EKSViewer`: Read-only access to workloads

---

## Prerequisites

- An existing EKS cluster with both API & ConfigMap access enabled
- IAM Identity Center set up and enabled
- AWS CLI v2 and `kubectl` installed
- User has access to the AWS Access Portal

---

## Step-by-Step Guide

---

### Step 1: Enable IAM Identity Center (if not yet done)

1. Go to the AWS Console
2. Search for **IAM Identity Center** > **Enable**
3. Choose the default directory or integrate with your IdP (Okta, Azure AD, etc.)
4. Create Users and Groups (e.g., `eks-admins`, `eks-developers`, `eks-viewers`)

---

### Step 2: Create Permission Sets

Go to **IAM Identity Center > Permission Sets > Create permission set**

#### 1 EKSAdmin (full access)

- **Name**: `EKSAdmin`
- **Type**: Custom
- **Policy**: Attach `AdministratorAccess` (AWS managed)

#### 2 EKSDeveloper

- **Name**: `EKSDeveloper`
- **Type**: Custom
- **Attach AWS managed policies**:
  - `AmazonEKSFullAccess`
  - `AmazonEC2ReadOnlyAccess`
- **(Optional)** Add inline policy for IAM listing/logs:

```json
{
  "Effect": "Allow",
  "Action": [
    "iam:ListRoles",
    "iam:GetRole",
    "logs:Describe*",
    "logs:Get*",
    "logs:List*"
  ],
  "Resource": "*"
}
````

#### 3 EKSViewer (read-only)

* **Name**: `EKSViewer`
* **Type**: Custom
* **Inline policy**:

```json
{
  "Effect": "Allow",
  "Action": [
    "eks:Describe*",
    "eks:List*",
    "ec2:Describe*",
    "logs:Get*",
    "logs:Describe*"
  ],
  "Resource": "*"
}
```

---

### Step 3: Assign Groups to AWS Account

Go to **IAM Identity Center > AWS Accounts > Assign users or groups**:

| Group            | Permission Set |
| ---------------- | -------------- |
| `eks-admins`     | `EKSAdmin`     |
| `eks-developers` | `EKSDeveloper` |
| `eks-viewers`    | `EKSViewer`    |

---

### Step 4: Map IAM Roles in `aws-auth` ConfigMap

Each permission set creates a unique IAM role like:

```
arn:aws:iam::<account-id>:role/AWSReservedSSO_<PermissionSetName>_<UUID>
```

Run:

```bash
kubectl edit configmap aws-auth -n kube-system
```

Add under `mapRoles`:

```yaml
mapRoles:
  - rolearn: arn:aws:iam::<account-id>:role/AWSReservedSSO_EKSAdmin_<UUID>
    username: eks-admin
    groups:
      - system:masters

  - rolearn: arn:aws:iam::<account-id>:role/AWSReservedSSO_EKSDeveloper_<UUID>
    username: eks-developer
    groups:
      - eks-developers

  - rolearn: arn:aws:iam::<account-id>:role/AWSReservedSSO_EKSViewer_<UUID>
    username: eks-viewer
    groups:
      - eks-viewers
```

---

### Step 5: Create Kubernetes RBAC Roles and Bindings

#### `eks-developers`

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: dev-role
rules:
- apiGroups: ["", "apps"]
  resources: ["pods", "services", "deployments"]
  verbs: ["get", "list", "create", "update", "delete"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: bind-developers
subjects:
- kind: Group
  name: eks-developers
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: dev-role
  apiGroup: rbac.authorization.k8s.io
```

#### `eks-viewers`

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: view-role
rules:
- apiGroups: [""]
  resources: ["pods", "services"]
  verbs: ["get", "list"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: bind-viewers
subjects:
- kind: Group
  name: eks-viewers
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: view-role
  apiGroup: rbac.authorization.k8s.io
```

Apply both:

```bash
kubectl apply -f dev-role.yaml
kubectl apply -f view-role.yaml
```

> `system:masters` group (for `EKSAdmin`) gives full access – no RBAC needed.

---

### Step 6: User Access & `kubectl` Setup

#### 1. User logs into [AWS Access Portal](https://<your-sso-url>.awsapps.com/start)

#### 2. Select account and permission set

#### 3. On local machine, run:

```bash
aws configure sso
aws sso login --profile eks-user
aws eks update-kubeconfig --region <region> --name <cluster-name> --profile eks-user
```

Then test:

```bash
kubectl auth can-i create pods       # Developer: ✅, Viewer: ❌
kubectl get pods                     # All roles: ✅
kubectl delete pod xyz              # Admin & Developer: ✅, Viewer: ❌
```

---

##  Summary

| Role      | IAM Identity Center Permission Set | Group          | Kubernetes Group | Access Level                 |
| --------- | ---------------------------------- | -------------- | ---------------- | ---------------------------- |
| Admin     | EKSAdmin (`AdministratorAccess`)   | eks-admins     | `system:masters` | Full cluster access          |
| Developer | EKSDeveloper                       | eks-developers | `eks-developers` | Create/update pods, services |
| Viewer    | EKSViewer (inline policy)          | eks-viewers    | `eks-viewers`    | View pods/services only      |

---

## 🔒 Security Notes

* Keep RBAC definitions in Git as code.
* Use permission sets instead of unmanaged inline IAM policies where possible.
* Periodically review access mappings in `aws-auth` and Identity Center.

```

---


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
