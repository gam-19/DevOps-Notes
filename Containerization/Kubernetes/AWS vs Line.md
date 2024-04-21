# Kubernetes in AWS vs Linode (IN PROGRESS)   

EKS (Elastic Kubernetes Service)
LKE (Linote Kubernetes Engine)

## Linode
* Linode sets up K8s cluster much faster, with few click you have a K8s cluster running.
  * In AWS takes more time because it has much more granual control of the infrastructure. There are more things you are able to configure: VPN, public and private subnets, 
  * Linode doesn't need to authenticate againta the platform, only to the cluster.
  * There are differente way to authenticate to K8s from Jenkins, it depends of what platform it's been used.
* Linote the K8s Control Plane is managed.
* You only have access and control the Worker nodes.


