# **_Kubernetes Cheat Sheet_**
    ## What isKubernetes?
       - Kubernetes is a platform for managing containerized workloads. 
       - Kubernetes orchestrates computing, networking and storage to provide a seamless portability across infrastructure providers.

# **_Cluster Info_**	
	
## List all services
 	kubectl get services                
## List all pods
	kubectl get pods                    
## Watch nodes continuously
	kubectl get nodes -w                
## Get version information
	kubectl version                     
## Get cluster information
	kubectl cluster-info                
## Get the configuration
	kubectl config view                 
## Output information about a node
	kubectl describe node <node>        
	
# **_Pod and Container Info**	
	
## List the current pods
	kubectl get pods                         
## Describe pod <name>
	kubectl describe pod <name>              
## List the replication controllers
	kubectl get rc                           
## List the replication controllers in <namespace>
	kubectl get rc --namespace="<namespace>" 
## Describe replication controller <name>
	kubectl describe rc <name>               
## List the services
	kubectl get svc                          
## Describe service <name>
	kubectl describe svc <name>              
	
# **_Interacting with Pods_**	
	
## Launch a pod called <name> using image <image-name>
 	kubectl run <name> --image=<image-name>                             
## Create a service described in <manifest.yaml>
 	kubectl create -f <manifest.yaml>                                   
## Scale replication controller <name> to <count> instances
 	kubectl scale --replicas=<count> rc <name>                          
## Map port <external> to port <internal> on replication controller <name>	                                                                    
 	kubectl expose rc <name> --port=<external> --target-port=<internal> 

# **_Stopping Kubernetes_**	
	
## Delete pod <name>
	kubectl delete pod <name>                                         
## Delete replication controller <name>
	kubectl delete rc <name>                                          
## Delete service <name>
	kubectl delete svc <name>                                         
## Stop all pods on <n>
	kubectl drain <n> --delete-local-data --force --ignore-daemonsets 
## Remove <node> from the cluster
	kubectl delete node <name>                                        
	
# **_Debugging_**	
	
## execute <command> on <service> selecting container <$container>
	kubectl exec <service> <command> [-c <$container>] 
## Get logs from service <name> selecting container <$container>	
	kubectl logs -f <name> [-c <$container>]           
## Watch the Kublet logs
	watch -n 2 cat /var/log/kublet.log                 
## Show metrics for nodes
	kubectl top node                                   
## Show metrics for pods
	kubectl top pod                                    
	
# **_Administration_**	
	
## Initialize your master node
	kubeadm init                                              
## Join a node to your Kubernetes cluster
	kubeadm join --token <token> <master-ip>:<master-port>    
## Create namespace <name>
	kubectl create namespace <namespace>                      
## Allow Kubernetes master nodes to run pods
	kubectl taint nodes --all node-role.kubernetes.io/master- 
## Reset current state
	kubeadm reset                                             
## List all secrets
	kubectl get secrets                                       
