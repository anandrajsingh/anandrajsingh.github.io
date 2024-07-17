**Networking Challenges**
Decoupled microservices based applications rely heavily on networking in order to mimic the tight-coupling once available in the monolithic era. Networking in general, is not the easiest to understand and implement. Kubernetes is no exception - as a containerized microservices orchestrator it needs to address a few distinct networking challenges.

- Container-to-Container communication inside Pods.
- Pod-to-Pod communication on the same node and across cluster nodes.
- Service-to-Pod communication within the same namespace and acrosss cluster namespaces.
- External-to-Service communication for clients to access applications in a cluster.


**Container-to-Container Communication Inside Pods**
Making use of underlying host operating system's kernel virtualization features, a container runtime creates an isolated network space for each container it starts. This isolated network space is referred as network namespace. A network namespace can be shared across containers, or with host operting system.

When a grouping of containers defined by a Pod is started, a special infrastructure <b>Pause Container</b> is initialized by the container runtime for the sole purpose of creating a network namespace for the Pod. All additional containers created through user requests, running inside the Pod will share the Pause container's network namespace so that they can all talk to each other via localhost.


**Pod-to-Pod Communication across Nodes**
The Kubernetes network model aims to reduce complexity, and it treats Pods as VMs on a network, where each VM is equipped with a network interface - thus each Pod recieving a unique IP address. This model is called "<b>IP-per-pd</b> and ensures Pod-to-Pod communication, just as VMs are able to communicate with each other on the same network.


**External-to-Pod Communication**
A successfully deployed containerized application running in Pods inside a Kubernetes cluster may require accessibility from the outside world. Kubernetes enables external accessibility through <b>Services</b>, complex encapsulations of network routing rule definitions stored in <b>iptables</b> on cluster nodes and implemented by <b>kube-proxy agents</b>. By exposing services to the external world with the aid of <b>kube-proxy</b>, applications become accessible from outside the cluster over a virtual IP address and a dedicated port number.