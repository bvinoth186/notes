
## List Cluster
ibmcloud ks clusters

## Show Configuration
ibmcloud ks cluster-config mycluster

## List Nodes
kubectl get nodes

## List Deployments
kubectl get deployments

## Create POD
kubectl run hello-world-python --image=docker.io/vinoth186/hello-world-python:v1

## Get POD
kubectl get pods

## Expose deployment
kubectl expose deployment/hello-world-python --type=NodePort --name=hello-world-python --port=4000 --target-port=4000

## Get Service
kubectl describe service hello-world-python

## Get Node
ibmcloud ks workers --cluster mycluster

## Delete Service and Deployment
kubectl delete deployments/hello-world-python services/hello-world-python 

## Delete POD
kubectl delete pods -l app=hello-world-python
