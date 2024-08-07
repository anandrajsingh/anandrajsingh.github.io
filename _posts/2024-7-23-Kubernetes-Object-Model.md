**Introduction**
Kubernetes became popular due to its advanced lifecycle management capabilities, implemented through a rich object model, representing different persistent entities in the Kubernetes cluster. Those entities describe:

- What containerized applications are running.
- The nodes where containerized applications are deployed.
- Application resource consumption.
- Policies attached to applications, like restart/upgrade policies, fault tolerance, ingress/egress, access control etc.

With each object, we declare our intent, or desired state of the object, in the <b>spec</b> section. The Kubernetes system manages the <b>status</b> section for objects, where it records the actual state of the object. At any given point in time, the Kubernetes Control Plane tries to match the object's actual state to desired state.

An object definition manifest must include other fields that specify the version of the API we are referencing as the <b>apiVersion</b>, the object type as <b>kind</b>, and additional data helpful to the cluster or users for accounting purposes - the <b>metadata</b>.

Examples of Kubernetes object types are Nodes, Namespaces, Pods, Replicasets, Deployements, Daemonsets etc.


**Nodes**
They are virtual identities assigned by Kubernetes to the systems part of cluster - whether Virtual Machines, bare metal, Containers etc. These identities are unique to each system, and are used by cluster for resources accounting and monitoring purposes, which helps with workload management throughout the cluster.

Each node is managed with the help of two Kubernetes node Agents - kubelet and kube-proxy, while it also hosts a container runtime. The container runtime is required to run all containerized workload on the node - control plane agents and user workloads. The kubelet and kube-proxy node agents are responsible for executing all local workload management related tasks - interact with the runtime to run containers, monitor containers and node health, report any issues and node state to the API Server, and manage network traffic to containers.

**Namespaces**
If multiple users and teams use the same Kubernetes cluster we can partition the cluster into virtual sub-clusters using Namespaces. The names of resources/objects created inside a Namespace are unique, but not across Namespaces in cluster.

Generally, Kubernetes creates four Namespaces out of the box:

- <b>kube-system</b>: contains the object and resources created by Kubernetes system, mostly the control plane agents.
- <b>kube-public</b> which is unsecured and readable by anyone, used for special purposes such as exposing public (non-sensitive) information about cluster.
- <b>kube-node-lease</b> which holds node lease objects used for node heartbeat data. Good practice, however, is to create additional namespaces, as desired, to virtualize the cluster and isolate users, developer teams, applications or tiers.

    Command to create namespace: kubectl create namespace `<namespace name>`

- <b>default</b> contains the object and resources created by administrators and developers, and objects are assigned to it by default unless another namespace name is provided by the user.


namespaces can be added in metadata section of yaml file, and imperative command to apply namespace is

- kubectl apply -f pod.yml --namespace=test

To switch your active namespace to test run command **kubens test**


**Pods**
A Pod is the smallest Kubernetes workload object. It is the unit of deployment in Kubernetes, which represents a single instance of the application. A Pod is a logical collection of one or more containers, enclosing and isolating them to ensure that they:

- Are scheduled togethor on the same host with the Pod.
- Shere the same network namespace, meaning that they share a single IP address originally assigned to the Pod.
- Have access to mount the same external storage (volumes) and other common dependencies.

Pods are ephemeral in nature, and they do not have the capabilty to self-heal themselves. That is the reason they are used with controllers, or operators(used interchangeably), which handle Pod's replication, fault tolerance, self-healing, etc. Examples of Controllers are Deployments, Replicasets, Daemonsets, Jobs etc.

**COMMANDS**

- **kubectl create -f `<yaml file name>`**
    Creates resouce defined in yaml file

- **kubectl get pods -o wide**
Gives detailed info about pods.

- **kubectl run nginx-pod --image=nginx:1.22.1 --port=80**
    Imperatively, we can simply run pod like above command

- **kubectl run nginx-pod --image=nginx:1.22.1 --port=80 \
--dry-run=client -o yaml > nginx.yaml**
Generates a definition manifest in YAML. It is a multiline command


**Labels**
Labels are key-value pairs attached to Kubernetes objects such as Pods, Replicasets, Nodes, Namespaces, and Persistent Volumes. Labels are used to organize and select a subset of objects, based on the requirements in place.

**Label Selectors**
Controllers and Services, use label Selectors to select a subset of object. Kubernetes supports two types of Selectors:

- **Equality-Based Selectors**
allow filtering of objects based on Label Keys and Values. Matching is using =, == (equals, used interchangeably), or != (not equals) operators.
- **Set-Based Selectors**
allow filtering of object based on a set of values. We can use **in, notin** operators for values, and **exist/does not exist** operator for label keys.


**ReplicationControllers**
Although no longer a recommended controller, a Replicationcontroller is a complex operator that ensures a specified number of replicas of a Pods are running at any given time, by constantly comparing the desired state with actual state.

However, the default recommended controller is the Deployement which configures a Replicaset controller to manage application Pod's lifecycle.


**Replicaset**
It is next generation of ReplicationController, as it implements the replication and self-healing aspects of the ReplicationController. Replicasets support both equality and set-based Selectors whereas ReplicationControllers only support equality-based Selectors.

Below is an example of a Replicaset object's definition manifest in YAML format.

apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: frontend
  labels:
    app: guestbook
    tier: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: guestbook
  template:
    metadata:
      labels:
        app: guestbook
    spec:
      containers:
      - name: nginx
        image: nginx:1.22.1


**kubectl scale rs frontend --replicas=4**


**Deployments**
Deployment objects provide declarative updates to Pods and Replicasets. The DeploymentController is part of the control plane node's controller manager, and as a controller it also ensures that current state always matches the desired state of our running containerized application. It allows for seamless application updates and rollbacks, known as the default **RollingUpdate** strategy, through **rollouts** and **rollbacks**, and it directly manages its Replicasets for application scaling, It also supports a disruptive, less popular strategy known as **Recreate**.

Below is an example of a Deployment Object's definition manifest in YAML format:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-deployment
  template:
    metadata:
      labels:
        app: nginx-deployment
    spec:
      containers:
      - name: nginx
        image: nginx:1.20.2
        ports:
        - containerPort: 80
        

**kubectl create deployment nginx-deployment \
--image=nginx:1.20.2 --port=80 --replicas=3**

A **rolling update** is triggered when we update specific properties of the Pod Template for deployment. While planned changes such as updating the container image, container port, volumes and mounts would trigger a new Revision, other operations that are dynamic in nature, like scaling or labelling the deployment, do not trigger the rolling update, thus do not change the Revision number.

Once the rolling update has completed, the **Deployment** will show both **Replicasets A and B**, where A is scaled to zero pods and B is scaled to 3 pods. This is how the deployment records its prior state configuration settings, as **Revisions**.

Some deployment commands:
**kubectl rollout status deploy nginx-deployment**
**kubectl rollout history deploy nginx-deployment**
**kubectl rollout history deploy nginx-deployment --revision=1**
**kubectl set image deploy nginx-deployment nginx=nginx:1.21.5 --record**
**kubectl rollout undo deploy nginx-deployment  --to-revision=1**
**kubectl get all -l app=nginx -o wide**
**kubectl get deploy, rs, po -l app=nginx**




**DAEMONSETS**
Daemonsets are operators designed to manage node agents. They resemble ReplicaSet and Deployment operators when managing multiple pod replicas and application updates, but the DaemonSets present a distinct feature that enforces a single Pod replica to be placed per Node, on all the Nodes or on a select subset of Nodes. In contrast, the ReplicaSet and Deployment operators by default have no control over the scheduling and placement of multiple Pod replicas on the same Node.

DaemonSet operators are commonly used in cases when we need to collect monitoring data from all Nodes, or to run storage, networking, or proxy daemons on all Nodes, to ensure that we have a specific type of Pod running on all Nodes at all times. They are critical API resources in multi-node Kubernetes clusters. The kube-proxy agent running as a Pod on every single node in the cluster, or the Calico or Cilium networking node agent implementing the Pod networking across all nodes of the cluster, are examples of applications managed by DaemonSet operators.

Whenever a Node is added to the cluster, a Pod from a given DaemonSet is automatically placed on it. Although it ensures an automated process, the DaemonSet's Pods are placed on all cluster's Nodes by the controller itself, and not with the help of the default Scheduler. When any one Node crashes or it is removed from the cluster, the respective DaemonSet operated Pods are garbage collected. If a DaemonSet is deleted, all Pod replicas it created are deleted as well.

The placement of DaemonSet Pods is still governed by scheduling properties which may limit its Pods to be placed only on a subset of the cluster's Nodes. This can be achieved with the help of Pod scheduling properties such as nodeSelectors, node affinity rules, taints and tolerations. This ensures that Pods of a DaemonSet are placed only on specific Nodes, such as workers if desired. However, the default Scheduler can take over the scheduling process if a corresponding feature is enabled, accepting again node affinity rules.
