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


