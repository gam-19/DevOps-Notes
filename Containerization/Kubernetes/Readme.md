EKS requires very specif VPC configuration, subnet, route table, etc, thefore we usually create a new VPC, and not use default VPC.

Steps
* Create **Control Plane: Master nodes**.
  - Highly available in 2 or 3 zones.
  Although AWS manage it, we need to create it.
* Create VPC
* We need to create Worker nodes using EC2 instances or Node Groups
* connect them to the Master nodes.
