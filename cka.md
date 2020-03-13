# CKA Notes

## Certification 

- Certified Kubernetes Administrator
  - https://www.cncf.io/certification/cka/

- Exam Curriculum (Topics) 
  - https://github.com/cncf/curriculum

- Candidate Handbook 
  - https://www.cncf.io/certification/candidate-handbook

- Exam Tips
  - http://training.linuxfoundation.org/go//Important-Tips-CKA-CKAD


## Cluster Architecture 

### K8s Architecture
- Master 
  - Manage 
  - Plan
  - Schedule 
  - Monitor nodes 
- Worker Nodes
  - Host application as Containers 
  - Minions 
	
- Master Side 	
  - ETCD Cluster 
    - DB its stores information in key value format of cluster data 
  - Kube-Scheduler 
    - Watches for newly created pods with no assigned node, and selects a node for them to run on.
  - Kube Controller Manager 
    - Control Plane component that runs controller processes.
    - Node Controller 
	  - Responsible for noticing and responding when nodes go down.
    - Replication Controller
	  - Responsible for maintaining the correct number of pods for every replication controller object in the system
	- Endpoints Controller 
	  - Populates the Endpoints object (that is, joins Services & Pods).
    - Service Account & Token Controllers
      - Create default accounts and API access tokens for new namespaces.
  - Kube-Api server 
    - For communication 
	- Kubernetes control plane that exposes the Kubernetes API. The API server is the front end for the Kubernetes control plane.
	
- Worker Side 	
  - Container Runtime Engine 
    - software that is responsible for running containers.
    - Docker 
	- Rocket (rkt)
	- Containerd
  - Kubelet
    - Agents runs on the node 
	- It makes sure that containers are running in a pod.
  - Kube-Proxy 
    - Communication between nodes
    - network proxy that runs on each node in your cluster, implementing part of the Kubernetes Service concept.	
	
- Addons 
  - DNS
    - Cluster DNS is a DNS server, in addition to the other DNS server(s) in your environment, which serves DNS records for Kubernetes services.
  - WebUI
    - Dashboard 
	- It allows users to manage and troubleshoot applications running in the cluster, as well as the cluster itself.
  - Container Resource Monitoring
    - Container Resource Monitoring records generic time-series metrics about containers in a central database, and provides a UI for browsing that data.
  - Cluster-level Logging
    - A cluster-level logging mechanism is responsible for saving container logs to a central log store with search/browsing interface.

- Pod 
  - A Pod is the basic execution unit of a Kubernetes application
  - the smallest and simplest unit in the Kubernetes object model that you create or deploy. 
  - A Pod represents processes running on your Cluster.
  - Each Pod is assigned a unique IP address. 
  - Every container in a Pod shares the network namespace, including the IP address and network ports. 
  - Containers inside a Pod can communicate with one another using localhost. 
  - A Pod can specify a set of shared storage Volumes. 
  - All containers in the Pod can access the shared volumes, allowing those containers to share data.
  - Pods that run a single container
    - one to one 
	- most common use case 
	- one container per pod
  - Pods that run multiple containers that need to work together.
    - A Pod might encapsulate an application composed of multiple co-located containers that are tightly coupled and need to share resources.

- Yaml
  - *.yaml or *.yml 
  - apiVersion
  - kind 
    - Pod  (apiversion: v1)
	- Namespace  (apiversion: v1)
	- ResourceQuoto (apiversion: v1)
	- Service (apiversion: v1)
	- ReplicaSet (apiversion: apps/v1)
	- RelicationController (apiversion: v1)
	- Deployment (apiversion: apps/v1)
  - metadata 
    - name
	- labels 
  - spec 
    - containers (array) 
	  - name 
	  - image 
	  
- RelicaSet
  - purpose is to maintain a stable set of replica Pods running at any given time
  - Load balancing and scaling 
  - In yaml 
  - spec
    - replicas 
	- selector
	  - matchLabeles 
	- template 
	  - embed the pod configuration 
	
- RelicationController
  - ensures that a specified number of pod replicas are running at any one time.
  - In yaml 
  - spec
    - replicas 
	- selector
	- template 
	  - embed the pod configuration 
  - difference between replica set and replication controller 
    - both are doing the same job
	- Replica Set use Set-Based selectors while replication controllers use Equity-Based selectors.
	- The replication controller makes sure that few pre-defined pods always exist. So in case of a pod crashes, the replication controller replaces it.
	- Replica sets are comparatively more useful. In recent times replication sets have replaced replication controllers.
	- supports matching labels
	
- Deployments
  - provides declarative updates for Pods and ReplicaSets.
	
- Namespaces 
  - default 
    - The default namespace for objects with no other namespace
  - kube-system 
    - The namespace for objects created by the Kubernetes system
  - kube-public 
    - This namespace is created automatically and is readable by all users
	- This namespace is mostly reserved for cluster usage
  - kubectl config set-context --current --namespace=<insert-namespace-name-here>
  - To validate 
    - kubectl config view --minify | grep namespace:
  - When you create a Service, it creates a corresponding DNS entry
    - service-name>.<namespace-name>.svc.cluster.local
	
- ResourceQuoto 
  - provides constraints that limit aggregate resource consumption per namespace
  
## Configuration 

- Commands 
  - JSON format
  - command - overrides  ENTRYPOINT in docker file
  - args - overrides CMD in docker file 

- ENV value type 
  - Environment variable 
  - key value pair 
  - spec section 
  - array 
  - env 
    - name 
	- value
	
	- valueFrom 
	  - configMapKeyRef
	  - secretKeyRef

	  
- Config Maps
  - allow you to decouple configuration artifacts from image content to keep containerized applications portable	  
  - 2 phases 
    - 1. creating config map 
	- 2. inject into the pod 
  - 2 ways of creating config map
  - 1. Imperative way 
    - kubectl create configmap <configname> <data-source>
	- kubectl create configmap <configname> --from-lieral=<key>=<value> 
	- kubectl create configmap <configname> --from-file=<path-to-file> 
  - 2. Declarative way
    - kubectl create -f map.yaml
	- apiVersion: v1
	- kind: ConfigMap
	- metadata:
	  -  name: app-config 
	- data:
	  - <data>
	- data tag instead of spec
  - 2 ways of injecting into pod 
    - 1. inject specific env value from config map to pod 
	- using valueFrom tag
	  - env 
		- name: <env_name> 
		- valueFrom: 
		  - configMapKeyRef: 
		    - name: <config-map_name)
			- key: <key from config map)
	- 2. inject all the key pair values from configmap into pod 
	- using envFrom tag 
	- envFrom 
      - configMapRef
	    - name: <configmap-name>
	 
- Secrets 
  - same as configmaps
  - this is to store confidential data 
  - creating secrets in imperative way   
    - kubectl create secret generic <secret-name> <data-source>
	- kubectl create secret generic <secret-name> --from-lieral=<key>=<value> 
	- kubectl create secret generic <secret-name> --from-file=<path-to-file> 
  - creating secrets in declarative way   
    - kubectl create -f secrets.yaml
	- apiVersion: v1
	- kind: Secret
	- metadata:
	  -  name: app-config 
	- data:
	  - <data>
	- data tag instead of spec
	- for Yaml data should be base64 encrypted 
	  - echo -n 'mysql' | base64
    - describe secrets will show only the key namespace
	- get secrets will show the encrypted value of key pair
	- To decrypt the values 
	  - echo -n 'bahbfdh=' | base64 --decode 
   - Secrets can be used As files in a volume mounted on one or more of its containers
	  
- Security Context
  - Docker Security 
    - containers and hosts shares same kernal 
	- its isolated by namespaces
	- docker container has its own namespace 
	- it cannot see outside of its namespace
	- with in the container if you list the
	  processes with ps aux command it list the 
	  processes running with in the container 
	- same ps aux command at the lost level also
	  list the same processes but with different 
	  process id
	- process isolation
	- by default docker runs processes as root user 
	- if you want enforce different user use --user command 
	- or USER in dockerfile 
	- docket limits the abilities of root user with in the container 
	- root user in the host and root user in container are not same
	  in terms of capability 
	- by default docker limits the capabilities of root user in 
	  container 
	- say for example you cant reboot the host 
	- to override 
	  - --cap-add MAC_ADMIN (to add permissions)
	  - --cap-drop KILL (to drop)
	  - for all the premissions use --privileged option 
  - Container Security 
    - Security Context 
	- can set the security at pod level or container level
	- if you set at pod level its carried to all the containers
	- if you set at container level it overrides the pod security 
	- in pod yaml 
	  - securityContext:
	    - runsAsUser: 1000
		- capabilities:
		  - add: ["MAC_ADMIN"]
    - to set at pod level have the securityContext under spec
	- to set at container level have the securityContext under containers tag
	
- Service Account
  - A service account provides an identity for processes that run in a Pod.
  - role based access controls (out of scope for CKA)
  - authentication and authorization 
  - 2 types of accounts
    - 1. user account 
	  - Developer accessing cluster to do the deployment
    - 2. Service account 
      - used by application to interact with k8s cluster
      - promotheous monitoring application
      - jenkins to do the deployment	  
  - kubectl create serviceaccount <name>
  - it creates the token 
  - token is stored as secret object 
  - to view secret --> describe secret 
  - Bearer token 
  - if the third party hosted in the same cluster 
    - automatically the mounting the secret token with in the pod as volume pod 
  - for every namespace has default service account 
  - by default will be using default service account (describe pod will show)
  - default service account is restricted supports only basic api queries 
  - should update the pod for new service account 
  - delete and recreate the pod
  - to  opt out of automounting API credentials for a pod, set automountServiceAccountToken to false in spec section
  - to not to mount the default secrete volume use automountServiceAccountToken attribute in spec
  
- Resource Requirements 
  - CPU
  - MEM
  - DISK
  - k8s scheduler places the pods into nodes based on pod and node capacity
  - if scheduler couldn't fit the pod into any node, pod will be in waiting state and Events it will show Insufficient cpu
  - by default k8s assumes pod or container require 0.5 CPU and 256 Mi MEM
  - By default k8s scheduler uses this number to identify the available node 
    - for defaults set LimitRange (kind: limitRange)
  - to overrides the default values in spce section of pod 
    - resources: 
	  - requests: 
	    - memory: "1Gi"
		- cpu: 1
  - cpu can be from 0.1 cpu
  - 0.1 is also represented as 100m 
  - m stands for milli
  - can go low as 1m, but less then that 
  - 1 CPU is equal to 
    - 1 AWS vCPU
	- 1 GCP Core
	- 1 Azure Core 
	- 1 Hyperthread 
  - with memory 
    - 256 Mi (mebi bytes)
	- or 268435456
	- 1G or 1M or 1K
	- 1Gi or 1Mi or 1 Ki
	- 1 Ki - 1 Kibibyte - 1024 bytes 
	- 1 Kb - 1 Kilobyte - 1000 bytes 
  - in docker world container has no limit to consume resource
  - lets say if container starts with 1 cpu and it can consume max available cpu of the node 
  - in k8s can limit this using limits section in resoucres
    - resources: 
	  - requests: 
	    - memory: "1Gi"
		- cpu: 1
	  - limits:
	    - memory: "2Gi"
		- cpu: 2
   - in case if resource exceeds its limit   
     - for cpu, k8s throttles 
	 - it cant use beyond the limit defined 
   - but with memory 
     - container can use more memory then limit 
	 - if it constantly uses more memory then it will be terminated
	 
- Taints and Tolerations
  - to set restrictions on what pod can be scheduled to what node 
  - no guaranteed to place pod on a particular node (Node affinity)
  - to ensure that pods are not scheduled onto inappropriate nodes. 
  - One or more taints are applied to a node
  - this marks that the node should not accept any pods that do not tolerate the taints
  - Tolerations are applied to pods, and allow (but do not require) the pods to schedule onto nodes with matching taints.
  - By default pods have no tolerations 
  - Opposite to Node affinity 
  - to add taint to node 
    - kubectl taint nodes node1 app=blue:NoSchedule
  - to remove taint from node 
    - kubectl taint nodes node1 app:NoSchedule-
  - to add toleration to pod, under spec 
    - tolerations:
	  - - key: "key"
        - operator: "Exists"
        - effect: "NoSchedule"  
	  - - key: "key"
		- operator: "Equal"
		- value: "value"
		- effect: "NoExecute"
		- tolerationSeconds: 3600
  - values are in double quotos 
  - its array, so can more then 1 tolerations
  - the operator is Exists (in which case no value should be specified)
  - the operator is Equal and the values are equal
  - effect, 3 types 
    - 1. NoSchedule
	  - new Pods will not be scheduled on the node 
	  - no effect of existing pods
	- 2. NoExecute
	  - any existing pods that do not tolerate the taint will be evicted immediately
	  -  pods that do tolerate the taint will never be evicted
	  - tolerationSeconds - dictates how long the pod will stay bound to the node after the taint is added.
	    - optional property 
	- 3. PreferNoSchedule
	  - “soft” version of NoSchedule 
	  – the system will try to avoid placing a pod that does not tolerate the taint on the node, but it is not guaranteed 
  - By default Master node is tainted automatically to avoid accepting the pods 
  - this can be changed and but recommended not to host the pods in master node 
  - to see this configuration 
    - kubectl describe node kubemaster | grep Taint
	
- Node Selector 
  - to limit a pod to place on a particular node 
  - for simple selection ( “AND or exact match”)
  - use label selectors to make the selection. 
  - in Pod 
    - spec section add 
	  - nodeSelector:
		- disktype: large 
    - in Node add this label and selectors 
	  - kubectl label nodes Node1 disktype=ssd
  
- Node Affinity 
  - it allows you to constrain which nodes your pod is eligible to be scheduled on, based on labels on the node
  - advanced expression 
    - OR
	- XOR
  - can add soft rule.  if the scheduler can’t satisfy it, the pod will still be scheduled
  - two types of affinity, 
    - 1. node affinity 
	- 2. inter-pod affinity or anti-affinity
  - two types of node affinity
    1. requiredDuringSchedulingIgnoredDuringExecution  (hard)
	2. preferredDuringSchedulingIgnoredDuringExecution  (soft)
	3. (planned) requiredDuringSchedulingRequiredDuringExecution
  - can be added to pod in spec section 
    - affinity:
      - nodeAffinity:
        - requiredDuringSchedulingIgnoredDuringExecution:
          - nodeSelectorTerms:
          - - matchExpressions:
            - - key: kubernetes.io/e2e-az-name
              - operator: In
              - values:
              - - e2e-az1
              - - e2e-az2
        - preferredDuringSchedulingIgnoredDuringExecution:
        - - weight: 1
          - preference:
            - matchExpressions:
            - - key: another-node-label-key
              - operator: In
              - values:
              - - another-node-label-value
    - operator In represents OR conditions.   
	- operator NotIn represents XOR conditions.   
	- operator Exists check whether the label presents with node  (value is not required)
	- 
  
  
  
  
  
- Commands 
  - kubectl run hello-world 
  - kubectl cluster-info 
  - kubectl get nodes 
  - kubectl create -f
  - kubectl apply -f 
  - kubectl edit pods
  - kubectl replace -f 
  - kubectl scale --replicas=5 replicaset new-replica-set
  - kubectl get all
  - kubectl get pods --all-namespaces
  - kubectl run redis --image=redis123 --generator=run-pod/v1 -l app=run
  - kubectl get pod my-pod -o yaml --export       # Get a pod's YAML without cluster specific information
  - kubectl delete pods <pod> --grace-period=0 --force
  - echo -n 'mysql' | base64
  - echo -n 'bahbfdh=' | base64 --decode 
  


  
	
