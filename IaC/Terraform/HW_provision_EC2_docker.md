1. Create custom **VPC**
2. Create custom **Subnet**  
    In one AZ
 ```HCL
HCL - Terraform

    provider "aws" {
        region = "us-east-2"
    }

    variable vpc_cidr_block {}
    variable subnet_cidr_block {}
    variable avail_zone {}
    variable env_prefix {}
    
    resource "aws_vpc" "myapp-vpc" {
        cidr_block = var.vpc_cidr_block
        tags = {
            Name: "${var.env_prefix}-vpc"
        }
    }
    
    resource "aws_subnet" "myapp-subnet-1" {
        vpc_id = aws_vpc.myapp-vpc.id
        cidr_block = var.subnet_cidr_block
        availability_zone = var.avail_zone
        tags = {
            Name: "${var.env_prefix}-subnet-1"
        }
    }
 ```
3. Connect VPC to Internet  
   Allow traffic in/out
   * Create **Route Table**  
   It's like a virtual Router in the VPC. Will forward traffic of the vpc.
   * Create **Internet Gateway**

    ```HCL
    HCL - Terraform

    resource "aws_route_table" "myapp-route-table" {
        vpc_id = aws_vpc.myapp-vpc.id
        route {
            cidr_block = "0.0.0.0/0"
            gateway_id = aws_internet_gateway.myapp-igw.id
        }
        tags = {
            Name: "${var.env_prefix}-rtb"
        }
    }

    resource "aws_internet_gateway" "myapp-igw" {
        vpc_id = aws_vpc.myapp-vpc.id
        tags = {
            Name: "${var.env_prefix}-igw"
        }
    }
    ```

4. Provision **EC2 Instance**
5. Deploy nginx Docker container
6. Create **Security Group** (Firewall)
   * Allow access to web server from Internet.
   * Allow SSH connecting for mgmt.
7. asdfsaf

Notes:  
**Security Group**: Firewall at server level.  
**NACL (Network access list)**: Firewall at subnet level.