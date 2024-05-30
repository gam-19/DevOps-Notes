# Automate provisioning EKS cluster with Terraform

Manual EKS cluster provisioning drawbacks: 
* Involves creating and configuring many componentes. 
* No version control (change history)
* No simple replication of infra possible
  - To another aws account, to another develop environment (stage, dev, test, prod).
* No simple clean up
* Difficult Team collaboration

![alt text](/IaC/Terraform/_terra-images/Manual_EKS_setup.png)

**Terraform** is currently the **best way to provision an EKS cluster!**
Most efficiente way for creation and management EKS cluster.

Steps:
* Create VPC module
  - Enable nat gateway
  - Enable single nat gateway
  - Enable dns hostnames
  - Set tags to help Control Plane process like CCM (cloud control manager) to know what vpc, subnets (resources) belong to the cluster.
  AWS VPC Terraform module in Terraform Registry.
* Create subnets
  1 private and 1 public pero availability zone.
  E.g. us-east-2 has 3 az, there 6 subnets.
  Private subnets for Workload.
  Public subnets for external resources like loadbalancer.
* 
