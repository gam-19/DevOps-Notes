#### IaC Tools
    Categories:
       - Ad hoc scripts.
       - Configuration management tools (Ansible, Puppet & Chef)
       - Server template tools 
       - Orchestration tools (Kubernetes)
       - Provisitioning tools:  
          * Cloud specific: AWS Cloud Formation, Azure Resource Manager, Google Cloud Deployment Manager.
          * Cloud Agnostic: Terraform, Pulumi
  

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