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