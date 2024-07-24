**Introduction**
Kubernetes became popular due to its advanced lifecycle management capabilities, implemented through a rich object model, representing different persistent entities in the Kubernetes cluster. Those entities describe:

- What containerized applications are running.
- The nodes where containerized applications are deployed.
- Application resource consumption.
- Policies attached to applications, like restart/upgrade policies, fault tolerance, ingress/egress, access control etc.

With each object, we declare our intent, or desired state of the object, in the <b>spec</b> section. The Kubernetes system manages the <b>status</b> section for objects, where it records the actual state of the object. At any given point in time, the Kubernetes Control Plane tries to match the object's actual state to desired state.

An object definition manifest must include other fields that specify the version of the API we are referencing as the <b>apiVersion</b>, the object type as <b>kind</b>, and additional data helpful to the cluster or users for accounting purposes - the <b>metadata</b>.

Examples of Kubernetes object types are Nodes, Namespaces, Pods, Replicasets, Deployements, Daemonsets etc.

**Namespaces**
If multiple users and teams use the same Kubernetes cluster we can partition the cluster into virtual sub-clusters using Namespaces. The names of resources/objects created inside a Namespace are unique, but not across Namespaces in cluster.

Generally, Kubernetes creates four Namespaces out of the box:

- <b>kube-system</b>: contains the object and resources created by Kubernetes system, mostly the control plane agents.
- <b>kube-public</b> which is unsecured and readable by anyone, used for special purposes such as exposing public (non-sensitive) information about cluster.
- <b>kube-node-lease</b> which holds node lease objects used for node heartbeat data. Good practice, however, is to create additional namespaces, as desired, to virtualize the cluster and isolate users, developer teams, applications or tiers.

    Command to create namespace: kubectl create namespace `<namespace name>`

- <b>default</b> contains the object and resources created by administrators and developers, and objects are assigned to it by default unless another namespace name is provided by the user.


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