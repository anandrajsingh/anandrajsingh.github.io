**KUBERNETES**

Kubernetes is a platform for managing containerized workloads and services, that facilitates both declarative configuration and automation. It has a large, rapidly growing ecosystem. It helps in management, deployement and scaling of containerized applications.

We can manually maintain a couple of containers or write scripts to manage the lifecycle of dozens of containers, orchestrators make things much easier for users especially when it comes to managing hundreds or thousands of containers running on a global infrastructure.

Most container orchestrators can:
- Group hosts together while creating a cluster, in order to leverage the benefits of dictributed systems.
- Schedule containers to run on hosts in the cluster based on resources availability.
- Enable containers in a cluster to communicate with each other regardless of the host they are deployed to in the cluster.
- Bind containers and storage resources.
- Group sets of similar containers and bind them to load-balancing constructs to simplify access to containerized applications by creating an interface, a level of abstraction between the containers and the client.
- Manage and optimize resource usage.
- Allow for implementation of policies to secure access to applications running inside containers.


With all these configurable yet flexible features, container orchestrators are an obvious choice when it comes to managing containerized applications at scale.

Kubernetes offers a very rich set of features for container orchestration. Some of its fully supported features are:

**Automatic bin packing**
Kubernetes automatically schedules containers based on resource needs and constraints, to maximize utilization without sacrificing availability.

**Designed for extensibility**
A Kubernetes cluster can be extended with new custom features without modifying the upstream source code.

**Self-healing**
Kubernetes automatically replaces and reschedules containers from failed nodes. It terminates and then restarts containers that become unresponsive to health checks, based on existing rules/policy. It also prevents traffic from being routed to unresponsive containers.

**Horizontal scaling**
Kubernetes scales applications manually or automatically based on CPU or custom metrics utilization.

**Service discovery and load balancing**
Containers receive IP addresses from Kubernetes, while it assigns a single Domain Name System (DNS) name to a set of containers to aid in load-balancing requests across the containers of the set.


Additional fully supported Kubernetes features are:

**Automated rollouts and rollbacks**
Kubernetes seamlessly rolls out and rolls back application updates and configuration changes, constantly monitoring the application's health to prevent any downtime.

**Secret and configuration management**
Kubernetes manages sensitive data and configuration details for an application separately from the container image, in order to avoid a rebuild of the respective image. Secrets consist of sensitive/confidential information passed to the application without revealing the sensitive content to the stack configuration, like on GitHub.

**Storage orchestration**
Kubernetes automatically mounts software-defined storage (SDS) solutions to containers from local storage, external cloud providers, distributed storage, or network storage systems.

**Batch execution**
Kubernetes supports batch execution, long-running jobs, and replaces failed containers.

**IPv4/IPv6 dual-stack**
Kubernetes supports both IPv4 and IPv6 addresses.