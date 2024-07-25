Kubernetes provides a higher-level abstraction called <b>Service</b>, which logically groups Pods and defines a policy to access them. This grouping is achieved via <b>Labels</b> and <b>Selectors</b>. Labels and Selectors use a <b>key-value</b> pair format.

Services can expose single Pods, Replicasets, Deployments, Daemonsets and Statefulsets. When exposing the Pods managed by operator, the Service's Selector may use the same labels as operator. A clear benefit of a Service is that it watches application Pods for any changes in count and their respective IP addresses while automatically updating the list of corresponding endpoints. Even for a single-replica application, run by a single Pod, the Service is beneficial during self-healing (replacement of a failed Pod) as it immediately directs traffic to the newly deployed healthy Pod.

apiVersion: v1
kind: Service
metadata:
  name: frontend-svc
spec:
  selector:
    app: frontend
  ports:
  - protocol: TCP
    port: 80
    targetPort: 5000


**kubectl get service, endpoints frontend-svc**


**kube-proxy**
Each cluster node runs a daemon called kube-proxy, a node agent that watches the API server on the control plane node for addition, updates and removal of Services and endpoints. It is responsible for implementing the service configuration on behalf of an administrator or developer, in order to enable traffic routing to an exposed application running in Pods. 

**Traffic Policies**
The kube-proxy node agent together with the iptables implement the load-balancing mechanism of the Service when traffic is being routed to the application Endpoint. Due to restricting characterstics of the iptables this load-balancing is random by default. This means that the Endpoint Pod to recieve the request forwarded by the Service will be randomly selected out of many replicas. This mechanism does not guarantee that the selected receiving Pod is the closest or even on the same node as the requester, therefore not the best mechanism. Since this the iptables supported load-balancing mechanism, if we desire better outcomes, we would need to take advantage of traffic policies.

Traffic policies allow users to instruct the kube-proxy on the context of traffic routing. The two options are Cluster and Local:

- The **Cluster** option allows kube-proxy to target all ready Endpoints of the Service in load-balancing process. This is default behaviour of the Service even when the traffic policy property is not expicitly declared.
- The **Local** option, however, isolates the load-balancing process to only include Endpoints of the Service located on the same node as the requester Pod, or perhaps the node that captured the inbound external traffic on its Nodeport.
While this might sound like ideal option, it does have a shortcoming - if the Service does not have ready endpoint on the node where the requester Pod is running, the Service will not route the traffic to Endpoint on other nodes to satisfy the request because it will be dropped by kube-proxy.


apiVersion: v1
kind: Service
metadata:
  name: frontend-svc
spec:
  selector:
    app: frontend
  ports:
    - protocol: TCP
      port: 80
      targetPort: 5000
  internalTrafficPolicy: Local
  externalTrafficPolicy: Local


**Service Discovery**
As Service are primary mode of communication between containerized applications managed by Kubernetes, it is helpful to be able to discover them at runtime. Kubernetes supports two methods for discovering Services:

**Environment Variables**
As soon as the Pod starts on any worker node, the **kubelet** daemon running on the node adds a set of environment variables in the Pod for all active Services. For example, if we have an active Service called **redis-master**, which exposes port **6379** and its **ClusterIP** is **172.17.0.6**, then, on newly created Pod, we can see the following environment variable:

**REDIS_MASTER_SERVICE_HOST=172.17.0.6
REDIS_MASTER_SERVICE_PORT=6379
REDIS_MASTER_PORT=tcp://172.17.0.6:6379
REDIS_MASTER_PORT_6379_TCP=tcp://172.17.0.6:6379
REDIS_MASTER_PORT_6379_TCP_PROTO=tcp
REDIS_MASTER_PORT_6379_TCP_PORT=6379
REDIS_MASTER_PORT_6379_TCP_ADDR=172.17.0.6**

With this solution we have to be careful while ordering our Services, as the Pods will not have environment variables set for Service which are created after the Pods are created.


**DNS**
Kubernetes has add-on for DNS, which creates DNS record for each Service and its format is **my-svc.my-namespaces.svc.cluster.local**. Services within the same Namespace find other Services just by names. If we add a Service **redis-master** in **my-ns** Namespace, all Pods in the same **my-ns** namespace, lookup the Service just by its name, **redis-master**. Pods from other namespaces, such as **test-ns**, lookup the same service by adding the respective Namespace as Suffix, such as **redis-master.my-ns** or providing FQDN of the service as **redis-master.my-ns.svc.cluster.local**

This is most common and highly recommended solution. For example, in the previous section's image, we have seen that an internal DNS is configured, which maps our Services **frontend-svc** and **db-svc** to **172.17.0.4** and **172.17.0.5** IP addresses respectively.

If we had a client application accessing the frontend application, the client would only need to "know" the frontend application's Service name and port, which are frontend-svc and port 80 respectively. From a client application Pod we could possibly run the following, allowing for the cluster internal name resolution and the kube-proxy to guide the client's request to a frontend Pod:

**kubectl exec client client-app-pod-name -c client-container-name -- /bin/bash -c curl -s frontend-svc:80**

**ServiceType**
Access scope is defined by ServiceType property, defined when creating the Service.

**ClusterIP**
It is the default ServiceType. A Service recieves Virtual IP address, known as its ClusterIp. This Virtual IP address is used for communicating with the Service and is accessible only from within the cluster.

`apiVersion: v1`
`kind: Service`
`metadata:`
`  name: frontend-svc`
`spec:`
`  selector:`
`    app: frontend`
`  ports:`
`  - protocol: TCP`
`    port: 80`
`    targetPort: 5`
`  type: ClusterIP`


**NodePort**
With the NodePort ServiceType, in addition to a ClusterIP, a high-port, dynamically picked from the default range **30000-32767**, is mapped to the respective Service, from all the worker nodes. For example, if mapped NodePort is **32233** for the service **frontend-svc**, then, if we connect to any worker node on port **32233**, the node will redirect all the traffic to assigned ClusterIP - **172.17.0.4**. If we prefer a specific high-port number instead, then we can assign that high-port number to the NodePort from the default range when creating the Service.

The Nodeport ServiceType is useful we want to make our Services accessible from external world. The end-user connects to any worker node on specified high-port, which proxies the request internally to the ClusterIp of the Service, then the request is forwarded to the applications running inside cluster. Let's not forget that the Service is load balancing such requests, and only forwards the request to one of the Pods running the desired application. To manage access to multiple application Services from external world, adminitrators can configure a reverse-proxy - an ingress, and define rules rules that target specific services within the cluster.

`apiVersion: v1`
`kind: Service`
`metadata:`
`  name: frontend-svc`
`spec:`
`  selector:`
`    app: frontend`
`  ports:`
`  - protocol: TCP`
`    port: 80`
`    targetPort: 5000`
`    nodePort: 32233`
`  type: NodePort`


**kubectl expose deploy frontend --name=frontend-svc \
--port=80 --target-port=5000 --type=NodePort**

**kubectl create service nodeport frontend-svc \
--tcp=80:5000 --node-port=32233**



**LoadBalancer**
With the LoadBalancer ServiceType:

- NodePort and ClusterIp are automatically created, and external load balancer will route to them.
- The Service is exposed at a static port on each worker node.
- The Service is exposed externally using the underlying cloud provider's load balancer feature.

The LoadBalancer ServiceType will only work if the underlying infrastructure supports the automatic creation of Load Balancers and have the respective support in Kubernetes, as is the case with the Google Cloud Platform and AWS. If no such feature is configured, the LoadBalancer field is not populated, it remains in Pending state, but the Service will still work as a typical NodePort type Service.


**ExternalIP**
A Service can be mapped to an ExternalIP address if it can route to one or more of the worker nodes. Traffic that is ingressed into the cluster with the ExternalIP (as destination IP) on the Service port, gets routed to one of the Service endpoints. This type of service requires an external cloud provider such as Google Cloud Platform or AWS and a Load Balancer configured on the cloud provider's infrastructure.

ExternalIPs are not managed by Kubernetes. The cluster administrator has to configure the routing which maps the ExternalIP address to one of the nodes.

**ExternalName**
It is a special ServiceType that has no Selectors and does not define any endpoints. When accessed within the cluster, it returns a CNAME record of externally configured Service.

The primary use case of this ServiceType is to make externally configured Services like my-database.example.com available to applications inside the cluster. If the externally defined Service resides within the same Namespace, using just the name my-database would make it available to other applications and Services within that same Namespace.


**Multi-Port Services**
A service report can expose multiple ports at the same time if required. Its configuration is flexible enough to allow for multiple grouping of ports to be defined in the manifest. This is a helpful feature when exposing Pods with one container listening on more than one port, or when exposing Pods with multiple containers listening on one or more ports.

A multi-port Service manifest is provided below:

apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: myapp
  type: NodePort
  ports:
  - name: http
    protocol: TCP
    port: 8080
    targetPort: 80
    nodePort: 31080
  - name: https
    protocol: TCP
    port: 8443
    targetPort: 443
    nodePort: 31443



**Port-Forwarding**
In Kubernetes, the port forwarding feature allows users to easily forward a local port to an application port. The application port can be a Pod container port, a Service port, and even a Deployment container port (from its Pod template)

**kubectl port-forward deploy/frontend 8080:5000**

**kubectl port-forward frontend-77cbdf6f79-qsdts 8080:5000**

**kubectl port-forward svc/frontend-svc 8080:80**