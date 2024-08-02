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
  *  Worker node types: Self-managed EC2 instances, semi-managed Node Groups, or managed 'Fargate'.

![alt text](/IaC/Terraform/_terra-images/AWS_EKS_gral_arch.png)

Steps:
1. Creating VPC module
   * In AWS Mgmt Console, we use Cloud Formation template to create VPC for EKS.
   * EKS requires very specific VPC setup, with subnets, route tables, etc.
   * Since we want to use Terraform instead Cloud Formation, then we can use a ready TF Module to create VPC for EKS. 

      1.1 Create a root 'vpc.tf' file in the project dir:  
        - Use [TF module code](https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws/latest?tab=inputs)
        - Define vpc attributes  
        - Subnets will be created as part of the module, however we can decide how many subnets, types, and cidr addresses.  
           Best practice:  
           - 1 private and 1 public per availability zone.
           E.g. us-east-2 has 3 az, then 6 subnets.
           - Private subnets for Workload.
           - Public subnets for external resources like loadbalancer. 
        - Set AZs dinamically depending on the region.
           - Define AZs name where subnets will be distribuited, querying Regions AZs.
        
        ```bash
          # Define region and provider where AZs will be created.
          provider "aws" {
            region = "us-east-2"
          }
          
          # Variables definition
          variable "vpc_cidr_block" {}
          variable "private_subnet_cidr_blocks" {}
          variable "public_subnet_cidr_blocks" {}
          
          # Querying region AZs
          data "aws_availability_zones" "azs" {}

          # We decide the vpc module name, this case "myapp-vpc"
          module "myapp-vpc" {
            source  = "terraform-aws-modules/vpc/aws"
            version = "5.1.2"

            #Name of the vpc resource
            name = "myapp-vpc"

            cidr = "var.vpc_cidr_block"

            #Subnets type and cidr
            private_subnets = var.private_subnet_cidr_blocks
            public_subnets = var.public_subnet_cidr_blocks            

            # Set (query) AZs name dinamically
            azs = data.aws_availability_zones.azs.names
          }
        ```
      ```bash
         # terraform.tfvars:
         vpc_cidr_block = "10.0.0.0/16"
         private_subnet_cidr_blocks = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
         public_subnet_cidr_blocks = ["10.0.4.0/24", "10.0.5.0/24", "10.0.6.0/24"]
          
      ```
      ```bash
         # providers.tf:
         terraform {
            required_providers {
              aws = {
                source = "hashicorp/aws"
                version = "5.20.1"
              }
            }
          }          
      ```      
      - Adding other attributes:
        - Enable nat gateway  
        NAT is required for worker nodes, that are in a different vpc, to be able to communicate with  control plane that is another vpc.
        - Enable single nat gateway
        - Enable dns hostnames
        - Set required tags to help Control Plane process like CCM (cloud control manager) to identify what resouces (vpc, subnets, etc) belong to our specific cluster.

      ```bash
         # Continuing in vpc.tf:
         # !.... code ...

         # Nat is enabled by dafault, we want to make it explicit
         enable_nat_gateway = true
         
         # By default one gateway per subnet is created, we want all private subnets route their internet traffic through this 'single shared NAT gateway'
         single_nat_gateway = true

         # When EC2 instances are created beside private and public ip addresses we want hostnames also be assigned.
         enable_dns_hostnames = true

            tags = {
                "kubernetes.io/cluster/myapp-eks-cluster" = "shared"
            }
                
            public_subnet_tags = {
                "kubernetes.io/cluster/myapp-eks-cluster" = "shared"
                "kubernetes.io/role/elb" = 1
            }

            private_subnet_tags = {
                "kubernetes.io/cluster/myapp-eks-cluster" = "shared"
                "kubernetes.io/role/internal-elb" = 1
            }

      ```
        
2. Creating EKS cluster  
   AWS EKS Module in terraform registry will create all required resources for the EKS cluster: Control Plan Nodes, Worker Nodes and K8 parts.

   * Create  'eks.tf' file  
   * Define some eks attributes
     * Eks module version
     * Cluster name
     * Cluster (K8s) version
     * Subnet and VPC IDs
     * Work nodes (node  groups)
         
    ```bash         
        # From certain version EKS need security group created by us.            
        resource "aws_security_group" "eks_cluster_sg" {
            name        = "eks-cluster-sg"
            description = "Security group for EKS cluster"
            vpc_id      = module.myapp-vpc.vpc_id

            ingress {
                from_port   = 443
                to_port     = 443
                protocol    = "tcp"
                cidr_blocks = ["0.0.0.0/0"]
            }

            egress {
                from_port   = 0
                to_port     = 0
                protocol    = "-1"
                cidr_blocks = ["0.0.0.0/0"]
            }
        }

        module "eks" {
            source  = "terraform-aws-modules/eks/aws"
            version = "20.20.0"
        
        # cluster name, has to be same name used in 'vpc.tf'
        cluster_name = "myapp-eks-cluster"
        cluster_version = "1.27"

        # Using vpc module ouputs 'private_subnet'
        subnet_ids = module.myapp-vpc.private_subnets
        vpc_id    = module.myapp-vpc.vpc_id

        # Tags are nor required, but good to use them.
        tags = {
            environment = "development"
            application = "myapp"
        }

        # Refer sg created previously
        cluster_security_group_id = aws_security_group.eks_cluster_sg.id

        # Using mananged node groups
        eks_managed_node_groups = {
            dev = {
                # Starting on 1.30, AL2023 is the default AMI type for EKS managed node groups
                ami_type       = "AL2023_x86_64_STANDARD"
                instance_types = ["t2.micro"]

                min_size     = 1
                max_size     = 3
                desired_size = 2
            }
        }
    }
    ```

       * asdfsadfa  
       * asdfasdf
       * 






* TF Modules creates over 50 required resources.
 We dont need to define individual resources, we just need to define two modules, passed the parameters we need, this way having high level configuration, which allows us to tweak with parameters. 

## Required settings to connect using Kubectl

### Step 1: Verify AWS CLI Credentials

In AWS Mgmt Console ensure that your AWS CLI is configured with the correct credentials. Run the following command to check the current configuration:

```powershell
aws configure list
```

### Step 2: Verify IAM Role Permissions

Ensure that the IAM role you are using has the necessary permissions. The role should have the following policies attached:
- `AmazonEKSClusterPolicy`
- `AmazonEKSWorkerNodePolicy`
- `AmazonEKS_CNI_Policy`
- `AmazonEC2ContainerRegistryReadOnly`

### Step 3: Update kubeconfig

Run the following command to update your kubeconfig file with the correct context and credentials:

```powershell
aws eks update-kubeconfig --name myapp-eks-cluster --region us-east-2
```

### Step 4: Verify kubeconfig Context

Ensure that the kubeconfig file is using the correct context. Run the following command to check the current context:

```powershell
kubectl config current-context
```

### Step 5: Add user (in our case 'gam') to EKS Cluster
EKS Cluster > Access > add Access Entry, select user, give EKS Admin, EKS Cluster Admin.

### Step 6: Test kubectl Access

Try to list the nodes in your EKS cluster to verify access:

```powershell
kubectl get nodes
```


```

By following these steps, you should be able to resolve the "Unauthorized" error and access your EKS cluster. If the issue persists, please provide any error messages or additional context for further troubleshooting.

EKS PRICING
- 0.10 $us/hr + EC2 running.