# AWS EKS Cluster
## CONCEPTS 
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
* Create **Control Plane: Master nodes**.

