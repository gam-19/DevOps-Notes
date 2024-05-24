# Automate provisioning EKS cluster with Terraform

Manual EKS cluster involves creating and configuring many componentes:
* No version control (change history)
* No simple replication of infra possible
  - Another aws account, another develop environment (stage, dev, test, prod).
* No simple clean up
* Difficult Team collaboration

![alt text](/IaC/Terraform/_terra-images/Manual_EKS_setup.png)

**Terraform** is currently the **best way to provision an EKS cluster!**
Most efficiente way for creation and management EKS cluster.
