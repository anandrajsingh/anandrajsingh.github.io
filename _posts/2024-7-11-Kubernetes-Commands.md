<p style="font-size: 20px;"><b>Some minikube Commands</b></p>

**minikube start**
Starts a local kubernetes cluster using Minikube
**minikube stop**	
Stops the local Kubernetes cluster that is running using Minikube
**minikube status**
Displays the status of local kubernetes cluster that is running using Kubernetes
**minikube delete**
Deletes the local kubernetes cluster that is running using Minikube
**minikube delete**
Displays the IP address of Minikube virtual machine
**minikube ssh**
Logs in to the Minikube virtual machine using SSH


<p style="font-size: 20px;"><b>Some kubectl Commands</b></p>

**kubectl version**   
Displays the version of kubectl and Kubernetes clustor components
**kubectl api-version**
List all the API version supported by cluster.
**kubectl explain**
Provides detailed information about a specific Kubernetes resource, including its field and description
**e.g.** kubectl explain pods --> This will give information about pod resource.
**kubectl get `<resource>`**   	    
Retrieves information about Kubernetes resources, such as Pods, Deployments and Services
**kubectl describe `<resource> <resource-name>`**   
Provides detailed information about a specific kubernetes resource or a set of kubernetes 
**e.g.** kubectl describe pod my-pod
**kubectl create**   
Creates new resources in kubernetes cluster
**e.g.** kubectl create -f pod.yml      -f is used when creating resource from file
**e.g.** kubectl create deployment nginx-deployment --image=nginx create resource from command line
**kubectl apply -f `<filename>`**   
Applies changes to a Kubernetes resource defined in configuration file 
**e.g.** kubectl apply -f deployment.yaml
**kubectl delete `<resource> <resource-name>`**   
Deletes a kubernetes resource 
**kubectl logs**   
Displays the logs of container.
**kubectl exec**   
Executes a command in container within a pod.
**e.g.** kubectl exec my-pod -- ls /app  -->  Executes ls /app in pod named my-pod.
**e.g.** kubectl exec my-pod -c my-container -- ls /app   -->  Executes command in specific container within a pod(if the pod has multiple container.)
**kubectl port-forward `<pod-name> <local-port>:<pod-port>`**
Used to forward one or more local port to a port on pod.
**e.g.** kubectl port-forward my-pod 8080:80  -->  This forwards local port 8080 to port 80 on the pod `my-pod`.
**kubectl expose `<resource> <resource-name> --port=<port> --target-port=<target-port>`**
To create a service that exposes a Kubernetes resource such as pod, deployement or replicaset, to make it accessible over the network.
**e.g.** kubectl expose deployment my-deployment --port=80 --target-port=8080   --> This exposes a service that exposes the deployment `my-deployment` on port 80, forwarding traffic to port 8080 on the pods.