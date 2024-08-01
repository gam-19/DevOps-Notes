#### IaC Tools

Historically, companies have been restricted to manual solutions for maintaining IT infrastructure—but Infrastructure as Code (IaC) offers a different solution.

With the rise of cloud and automation technology, as well as DevOps practices, it’s become simpler, more efficient, and reliable to provision complex infrastructure. Cloud providers like Amazon Web Services (AWS), Google Cloud Platform, and Microsoft Azure allow APIs to facilitate automation of resources in their environments. Your application’s underlying infrastructure and configuration can now be automated and versioned using code.

IaC enables writing and executing code to define, deploy, update, and destroy infrastructure. This model treats all aspects of operations as software—even those that represent hardware. Using IaC to define application environments, companies will be better equipped to eliminate the risks of configuration drift and accomplish more reliable outcomes in their architectures.

Source:
https://www.cloudforecast.io/blog/using-terraform-to-deploy-to-eks/




    Categories:
       - Ad hoc scripts.
       - Configuration management tools (Ansible, Puppet & Chef)
       - Server template tools 
       - Orchestration tools (Kubernetes)
       - Provisitioning tools:  
          * Cloud specific: AWS Cloud Formation, Azure Resource Manager, Google Cloud Deployment Manager.
          * Cloud Agnostic: Terraform, Pulumi
  


HELPFUL Links:  
Granting IAM User Access to Existing EKS Cluster  
https://medium.com/@abhimanyubajaj98/granting-iam-user-access-to-existing-eks-cluster-a-step-by-step-guide-2e377d6ee4de

Deploying Kubernetes from Scratch with Terraform  
https://medium.com/@abhimanyubajaj98/deploying-kubernetes-from-scratch-with-terraform-a-step-by-step-guide-7d628910efd0

AUTOMATE IAM ROLE MAPPING ON AMAZON EKS WITH HASHICORP TERRAFORM  
https://dev.ahead.com/resources/automate-iam-role-mapping-on-amazon-eks/



GRAL TERRAFORM USAGE PROCEDURE

* Setup Terraform 
  * Install, setup auth
* Connect TF to Platforms 
  * Use TF Providers
* Create resources
  * Create VPC resource
    * Cidr block
* Create subnet
  * vpc id
  * cidr block  
    Subnet ip range should inside vpc cidr blocks but not overlap with other subnet cird blocks.
  * az


TERRAFORM BEST PRACTICES
* Create Infrastructure **from scratch**.
* **Leave the default** created by AWS as is.
* Modularize TF config file
If there is a complex configuration with too many resources, huge config file, lack of config overview, is best practice to **modularize**  
  ![alt text](/IaC/Terraform/_terra-images/TF_bp_modules.png)
  You should know: 
  * How to write a module
  * How to use/reference a module