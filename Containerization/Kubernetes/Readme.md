### AWS EKS Cluster componentes

* **Control Plane with Master nodes**   
  Are managed by AWS itself, but still we need to created them.
* **Worker nodes**   
  We need to create them using EC2 instances or Node Groups and connect them to the Master nodes.
* EKS requires very specif VPC configuration, subnet, route table, etc, thefore we usually create a new VPC where the worker nodes will run, and not use default VPC.

We created EKS cluster in specific AWS region, which has multiple AZ, and that gives us higly available Control Plane and Worker nodes

Steps
* Create VPC with all subnets and require configurations
  - Using AWS Cloud formation template.
  - Terraform config file.
* Create **Control Plane: Master nodes**.
  - Highly available in 2 or 3 zones.
  
**Princing**
Considerations:  
AWS EKS cluster pricing 0.10 $us/hr.
