**Some minikube commands**

-minikube start
Starts a local kubernetes cluster using Minikube
-minikube stop		
Stops the local Kubernetes cluster that is running using Minikube
-minikube status	
Displays the status of local kubernetes cluster that is running using Kubernetes
-minikube delete	
Deletes the local kubernetes cluster that is running using Minikube
-minikube ip		
Displays the IP address of Minikube virtual machine
-minikube ssh		
Logs in to the Minikube virtual machine using SSH


**Some kubectl commands**

-kubectl version	    
Displays the version of kubectl and Kubernetes API server
-kubectl get		    
Retrieves information about Kubernetes resources, such as Pods, Deployments and Services
-kubectl describe	
Provides detailed information about a specific kubernetes resource
-kubectl create		
creates a new deployment
-kubectl apply < >	
Applies changes to a Kubernetes resource based on configuration file named<>
-kubectl delete		
Deletes a kubernetes resource 
-kubectl logs		
Displays the logs for a specific Pod named ‘my-pod’
-kubectl exec		
Executes a command
-kubectl port-forward	
Forward port 8080 on local machine to port 80 on a specific Pod 
kubectl expose		
