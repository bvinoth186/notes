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

# K8s Architecture
- Master 
  - Manage 
  - Plan
  - Schedule 
  - Monitor nodes 
- Worker Nodes
  - Host application as Containers 
	
- Master Side 	
  - ETCD Cluster 
    - DB its stores information in key value format 
  - Kube-Scheduler 
  - Kube Controller Manager 
    - Node Controler 
    - Replication Controller
  - Kube-Api server 
    - For communication 
	
- Worker Side 	
  - Container Runtime Engine 
    - Docker 
	- Rocket (rkt)
	- Containerd
  - Kubelet
    - Agents runs on the node 
  - Kube-Proxy 
    - Communication between nodes 
  
## ETCD 
  
	