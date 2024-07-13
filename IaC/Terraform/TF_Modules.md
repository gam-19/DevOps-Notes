# TERRAFORM MODULES
It is a group of multiple resources that are used together.  


## WHY?
Without modules, complex configurations in a huge file with no overview.  

Modulos allows:  
* Organize and group configurations.
* Encapsulate into logical components.
* Reusage
* Expose created resources and their specific attributes.  
    - E.g 1. Module for AWS EC2 instance, with its configurations around it: Key pair, security group, volumes, aws ami, etc.   
    - E.g 2. Module for VPC including subnet, internet gateway, route table.
    We can reuse that "Web server" module to deploy it in another Region, another environment (Staging, QA, Dev, Prod)

## HOW?
Using modules created by ourselves.
Using [existing](https://registry.terraform.io/browse/modules) modules created by Terraform or other providers.

Good practice for dividing modules files:
- main.tf
- variable.tf
- outputs.tf
- providers

Steps:  
Creating custom module
* Create "Modules" dir in root.
* Create dir for each module. E.g. Subnet, web-server.  
* Creates main.tf, output.tf and variables.tf in each module
* Write the code in main.tf
* Define all variables.tf required in variables.tf
* Use output.tf as needed.  
Using module in root main.tf file  
```bash
    module "myapp-subnet" {
    #Windows file system path
    source = ".\\modules\\subnet"
    subnet_cidr_block = var.subnet_cidr_block
    avail_zone = var.avail_zone
    env_prefix = var.env_prefix
    vpc_id = aws_vpc.myapp-vpc.id
    default_route_table_id = aws_vpc.myapp-vpc.default_route_table_id      
    }
```
Reference all variables required by the module, make sure variables value are in terraform.tfvars file or you can hardcode when calling th module.

