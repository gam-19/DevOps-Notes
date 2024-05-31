EKS requires very specif VPC configuration, subnet, route table, etc, thefore we usually create a new VPC, and not use default VPC.

Steps
* Create VPC with all subnets and require configurations
  - Using AWS Cloud formation template.
  - Terraform config file.
* Create **Control Plane: Master nodes**.
  - Highly available in 2 or 3 zones.
  Although AWS manage it, we need to create it.

* We need to create Worker nodes using EC2 instances or Node Groups
* connect them to the Master nodes.

Considerations:  
AWS EKS cluster pricing 0.10 $us/hr.
