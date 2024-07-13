# KUBERNETES
### WHAT IS?
Opensource container orchestration tool

### Introduction / context / Why to use K8s

  - Currently containerized applications are the prefered way to release apps.
  - Apps can have multiple containers, even more, hundreds of containers.
  - Trend from Monolith to Microservices, resulted in an increased usage of containers.
    - Containers are the perfect host for microservice apps.
  - Manually managing hundreds or hundreds of 1000s containers is a lot of effort.

### BENEFITS  
* K8s Automates many processes involved in deploying, managing and scaling containerized apps.
* Became **most used** container orchestration platform.
* K8s allows to automates many of those manual task, additionaly provides:
  * **High Availability** or no downtime.
  * Automatic **Scaling**
  * **Disaster Recovery** - Backup and Restore
  * **Self-Healing**

### HOW TO USE IT
### Components
Kubernetees cluster consists of machines called "Nodes"

Worker Nodes (servers)  
       
      * Run the containerized applications.
      * Each Node can run multiple Pods
      * Pods = abstraction of docker containers.
      * Requires more compute recources, because the actual workload runs on them.  
          
          How they work  
            Work nodes need to have 3 processess installed
            * a) Container runtime
                * Platform to run the containers: Containerd, CRI-O, Docker.
            * b) Kubelet
                * Communicates with  container runtime to start/stop containers in Pods, also to underlying server to get resources for Pods.  
                * Makes sure cointainers run in a Pod
            * c) Kube-proxy
                * Network proxy, intelligent forwarding of requests to Pods.


Control Plane Nodes
* **Manages** the Work Nodes and the Pods in the cluster.
* Much more important and needs to be replicated.  
            How they work  
              Control Plane need 4 processes installed  
              * a) API Server  
              * b) Scheduler  
              * c) Controller Manager  
              * d) Etcd  

You can run K8s cluster On-Premises or in the cloud.  
On-prem > Minikube  

* We need to install K8 client: Cli or Gui
* K8 client CLI: Kubectl
* We can create cluster, pods, replicasets, services manually.
* Or we can use Yaml files to CRUD K8 resources.

Cloud > Linode, AWS, Azure




### Components
* **Control Plane with Master nodes**   
  Are managed by AWS itself, but still we need to created them.
* **Worker nodes**   
  We need to create them using EC2 instances or Node Groups and connect them to the Master nodes.
* EKS requires very specif VPC configuration, subnet, route table, etc, thefore we usually create a new VPC where the worker nodes will run, and not use default VPC.

We created EKS cluster in specific AWS region, which has multiple AZ, and that gives us higly available Control Plane and Worker nodes

EKS is created over the AWS Mgmt console or eksctl tool actually use AWS Cloud formation (AWS IaC tool) that has templates with required configurations.


**Princing**
Considerations:  
AWS EKS cluster pricing 0.10 $us/hr.

## HANDS ON

1. Create VPC  
With all subnets and require configurations  
   - Using AWS Cloud formation template.
   - Using Terraform configuration file.
* Create **Control Plane**   
  Master nodes



### Deploying images from private Docker registry like Elastics Container Registry ECR .
  1. Create Secret component
      - Docker login manually and use config.json to create secret out of it.
  2. Configure Deployment/Pod
      - Use previous secret using 'imagePullSecrets' command.


Install Minikube  
https://minikube.sigs.k8s.io/docs/start/?arch=%2Fwindows%2Fx86-64%2Fstable%2F.exe+download

