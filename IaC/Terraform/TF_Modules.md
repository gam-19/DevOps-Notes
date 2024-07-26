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
* A module should be a proper logical abstraction of resources, meaning, it's not worth it to create on resource only, instead should be used to create a whole group of resources that are related (three to four resources). Ej. a webserver with all configurations around it, security group, key pair, volumes.  

## HOW?
Using modules created by ourselves.
Using [existing](https://registry.terraform.io/browse/modules) modules created by Terraform or other providers.

Good practice for dividing modules files:
- main.tf
- variables.tf
- outputs.tf
- providers

Steps:  
Creating custom module
* Create "Modules" dir in root.
* Create dir for each module. E.g. subnet, web-server.  
* Creates main.tf, output.tf, providers.tf and variables.tf in each module dir include root module.
* Copy code group of related resources to child module main.tf
* Update variables names in child module main.tf
  * Child module variables should all be listed in module's variables.tf
* Move all outputs to root outputs.tf
* Move all variables in root 'main.tf' to root 'variables.tf' 
* Call module from root main.tf
  * Specify modules location path
  * Pass values for all module's variables.
  * Make sure modules variables should be listed in module 'variables.tf', also in root 'variables.tf' and values are in 'terraform.tfvars', otherwise hardcoded or referenced in root main.tf. 
* Update variable references in root 'main.tf' to values that are modulized now.
* Run 'terraform init' before applying new config file.


### Copy group of related resources to child module main.tf and update varaibles
* Copy code or related resources to child main.tf in module 'subnet'
* Update variables names, all will be passed from root main.tf.
  
```bash
# child module: modules/subnet/main.tf
    resource "aws_subnet" "myapp-subnet-1" {
        #The following variable should be updated for another variable name in all ocurrences.
        #vpc_id = aws_vpc.myapp-vpc.id
        vpc_id = var.vpc_id
        cidr_block = var.subnet_cidr_block
        availability_zone = var.avail_zone
        tags = {
            Name: "${var.env_prefix}-subnet-1"
        }
    }

    resource "aws_internet_gateway" "myapp-igw" {
    #vpc_id = aws_vpc.myapp-vpc.id
    vpc_id = var.vpc_id
    tags = {
        Name: "${var.env_prefix}-igw"
    }
    }

    # follogin variable should also be updated it for another name.  
    #default_route_table_id = aws_vpc.myapp-vpc.default_route_table_id
    default_route_table_id = var.default_route_table_id
        route {
            cidr_block = "0.0.0.0/0"
            gateway_id = aws_internet_gateway.myapp-igw.id
        }
        tags = {
            Name: "${var.env_prefix}-main-rtb"
        }
    }

```

### Child module variables should all be in module's variables.tf
* All subnet module variables listed in '/module/subnet/variables.tf'

```bash
# module/subnet/variables.tf
    variable "vpc_id" {}
    variable "subnet_cidr_block" {}
    variable "avail_zone" {}
    variable "env_prefix" {}
    variable "default_route_table_id" {}
```


### Move root outputs to root 'outputs.tf'

```bash
# root module ./outputs.tf
    output "aws-ami-id" {
    value = data.aws_ami.latest-amazon-linux-image.id
    }

    output "ec2-public-ip" {
    value = aws_instance.myapp-server.public_ip
    }
```
### Move all variables in root 'main.tf' to root 'variables.tf'
```bash
# root module ./variables.tf content
    variable "vpc_cidr_block" {}
    variable "subnet_cidr_block" {}
    variable "avail_zone" {}
    variable "env_prefix" {}
    variable "my_ip" {}
    variable "instance_type" {}
    variable "public_key_location" {}
```
### Call module from root main.tf
  * Specify modules location path
  * Pass values for all module's variables.
  * Make sure modules variables should be listed in module 'variables.tf', also in root 'variables.tf' and values are in 'terraform.tfvars', otherwise hardcoded or referenced in root main.tf.

Using module in root main.tf file  
```bash
    # Call other module using word 'module' and asign a name.
    module "myapp-subnet" {
        # Specify referenced modules path
        #Windows file system path
        source = ".\\modules\\subnet"

        #Pass values for all variables defined in 'module/subnet/variables.tf', actual values are in root 'main.tf', or root 'terraform.tfvars' but then variables names must be listed in root 'variables.tf' as well. Otherwise value can be hardcoded here.
        subnet_cidr_block = var.subnet_cidr_block
        avail_zone = var.avail_zone
        env_prefix = var.env_prefix
        vpc_id = aws_vpc.myapp-vpc.id
        default_route_table_id = aws_vpc.myapp-vpc.default_route_table_id      
    }
```
  ![alt text](/IaC/Terraform/_terra-images/TF_modules_variables.png)

Reference all variables required by the module, make sure variables value are in terraform.tfvars file or you can hardcode when calling th module.

### Update variable references in root 'main.tf' to values that are modulized now.

* Accessing module resource from another module.
```bash
#Root 'main.tf' content before:
resource "aws_instance" "myapp-server" {
    ami = data.aws_ami.latest-amazon-linux-image.id
    instance_type = var.instance_type

    #Following variable value is not valid anymore, since is not in root 'main.tf' anymore.
    subnet_id = aws_subnet.myapp-subnet-1.id       
    
    vpc_security_group_ids = [aws_default_security_group.default-sg.id]
    availability_zone = var.avail_zone

    associate_public_ip_address = true

    key_name = aws_key_pair.ssh-key.key_name        
         
    user_data = file("entry-script.sh")

    user_data_replace_on_change = true

    tags = {
        Name: "${var.env_prefix}-server"
    }
}
```
* Output subnet object in module 'output.tf', exposes/exports resources attributes to parent module.
```bash
# Module's 'output.tf'
    output "subnet" {
        #exposes all subnet attributes
        value = aws_subnet.myapp-subnet-1
    }
```

* Reference resource from root 'main.tf'
```bash
#Root 'main.tf' content after:
resource "aws_instance" "myapp-server" {
    ami = data.aws_ami.latest-amazon-linux-image.id
    instance_type = var.instance_type

    #new subnet_id value
    subnet_id = module.myapp-subnet.subnet.id
    
    vpc_security_group_ids = [aws_default_security_group.default-sg.id]
    availability_zone = var.avail_zone

    associate_public_ip_address = true

    key_name = aws_key_pair.ssh-key.key_name        
         
    user_data = file("entry-script.sh")

    user_data_replace_on_change = true

    tags = {
        Name: "${var.env_prefix}-server"
    }
}
```