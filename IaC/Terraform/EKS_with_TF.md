# Automate provisioning EKS cluster with Terraform

### Manual EKS cluster provisioning drawbacks: 
* Involves creating and configuring many componentes, overwelming. 
* No version control (change history)
* No simple replication of infra possible
  - To another aws account, to another develop environment (stage, dev, test, prod).
* No simple clean up
* Difficult Team collaboration, to share knowledge with team. 

![alt text](/IaC/Terraform/_terra-images/Manual_EKS_setup.png)

### Using Terraform
* **Terraform** is currently the **best way to provision an EKS cluster!**
* Most efficiente way for creation and management EKS cluster.  
* More than half of all K8s cluster in the world are running in AWS.

### Gral componentes K8s needs to be created:
- Control Plane
  * AWS offers highly available Control Plane  
    mycluster.eks.aws.amazon.com
  * Managed by aws itself.
- Worker nodes
  *  We need to created worker nodes and connect them to the Control Plane nodes.
  *  AWS helps us to have highly available worker nodes too, having them deployed in Availability Zones.

![alt text](/IaC/Terraform/_terra-images/AWS_EKS_gral_arch.png)

Steps:
1. Creating VPC module
   * In AWS Mgmt Console, we use Cloud Formation template to create VPC for EKS.
   * EKS requires very specific VPC setup, with subnets, route tables, etc.
   * Since we want to use Terraform instead Cloud Formation, then we can use a ready TF Module to create VPC for EKS. 


      - Enable nat gateway
      - Enable single nat gateway
      - Enable dns hostnames
      - Set tags to help Control Plane process like CCM (cloud control manager) to identify what resouces (vpc, subnets, etc) belong to our specific cluster.
     AWS VPC Terraform module in Terraform Registry.
  * Create subnets  
     - 1 private and 1 public per availability zone.
     E.g. us-east-2 has 3 az, there 6 subnets.
     - Private subnets for Workload.
     - Public subnets for external resources like loadbalancer.* 

You'll have hight

EKS Cluster creation
- Control Plane

 TF Modules creates over 50 required resources.
 We dont need to define individual resources, we just need to define two modules, passed the parameters we need, this way having high level configuration, which allows us to tweak with parameters. 


EKS PRICING
- 0.10 $us/hr + EC2 running.