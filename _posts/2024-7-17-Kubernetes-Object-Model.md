Kubernetes became popular due to its advanced lifecycle management capabilities, implemented through a rich object model, representing different persistent entities in the Kubernetes cluster. Those entities describe:

- What containerized applications are running.
- The nodes where containerized applications are deployed.
- Application resource consumption.
- Policies attached to applications, like restart/upgrade policies, fault tolerance, ingress/egress, access control etc.

With each object, we declare our intent, or desired state of the object, in the <b>spec</b> section. The Kubernetes system manages the <b>status</b> section for objects, where it records the actual state of the object. At any given point in time, the Kubernetes Control Plane tries to match the object's actual state to desired state.

An object definition manifest must include other fields that specify the version of the API we are referencing as the <b>apiVersion</b>, the object type as <b>kind</b>, and additional data helpful to the cluster or users for accounting purposes - the <b>metadata</b>.

Examples of Kubernetes object types are Nodes, Namespaces, Pods, Replicasets, Deployements, Daemonsets etc.