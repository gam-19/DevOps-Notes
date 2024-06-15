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
   Using custom Route table to allow traffic in/out
   * Create **Route Table**  
   It's like a virtual Router in the VPC. Will forward traffic of the vpc.
   * Create **Internet Gateway**
   * Associate subnet to internet gateway.

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

    resource "aws_route_table_association" "a-rtb-subnet" {
        subnet_id = aws_subnet.myapp-subnet-1.id
        route_table_id = aws_route_table.myapp-route-table.id
    }
    ```
    Using default Route table   
    ```HCL
    HCL - Terraform
    
    resource "aws_internet_gateway" "myapp-igw" {
        vpc_id = aws_vpc.myapp-vpc.id
        tags = {
            Name: "${var.env_prefix}-igw"
        }
    }

    resource "aws_default_route_table" "main-rtb" {
        default_route_table_id = aws_vpc.myapp-vpc.default_route_table_id
        route {
            cidr_block = "0.0.0.0/0"
            gateway_id = aws_internet_gateway.myapp-igw.id
        }
        tags = {
            Name: "${var.env_prefix}-main-rtb"
        }
    }
    # Subnet assouciatin to route table happens automatically, no need to specify.    
    ```

4. Create **Security Group** (Firewall)
   * Allow access to web server from Internet.
   * Allow SSH connecting for mgmt.

    Using custom segurity group
    ```HCL
    resource "aws_security_group" "myapp-sg" {
        name = "myapp-sg"
        vpc_id = aws_vpc.myapp-vpc.id

        ingress {
            from_port = 22
            to_port = 22
            protocol = "tcp"
            cidr_blocks = var.my_ip
        }
        ingress {
            from_port = 8080
            to_port = 8080
            protocol = "tcp"
            cidr_blocks = ["0.0.0.0/0"]
        }
        egress {
            from_port = 0
            to_port = 0
            protocol = "-1"
            cidr_blocks = ["0.0.0.0/0"]
            prefix_list_ids = []
        }
        tags = {
            Name = "${var.env_prefix}-sg"
        }   
    }

    Using existing default segurity group
        
    ```HCL
    resource "aws_default_security_group" "default-sg" {
        vpc_id = aws_vpc.myapp-vpc.id

        ingress {
            from_port = 22
            to_port = 22
            protocol = "tcp"
            cidr_blocks = var.my_ip
        }
        ingress {
            from_port = 8080
            to_port = 8080
            protocol = "tcp"
            cidr_blocks = ["0.0.0.0/0"]
        }
        egress {
            from_port = 0
            to_port = 0
            protocol = "-1"
            cidr_blocks = ["0.0.0.0/0"]
            prefix_list_ids = []
        }
        tags = {
            Name = "${var.env_prefix}-default-sg"
        }   
    }
    ```

5. Provision **EC2 Instance**
   * Define ami image
   * Instance type
   * In which Subnet will be deployed
   * In which VPC Security Group
   * In which Availability Zone
   * Asociate Public IP
   * Create access keys and associate key name with EC2 instance
    You also have to download the private key .pem to yoour computer so you can ssh the EC2 instnace.

    ```bash
        # Inside main.tf
        
        # Create key pair resource
        resource "aws_key_pair" "ssh-key" {
            key_name = "server-key"
            public_key = file(var.public_key_location)
        }
        
        resource "aws_instance" "myapp-server" {
            ami = data.aws_ami.latest-amazon-linux-image.id
            instance_type = "t2.micro"
            
            subnet_id = aws_subnet.myapp-subnet-1.id            
            vpc_security_group_ids = [aws_default_security_group.default-sg.id]
            availability_zone = var.avail_zone
            
            associate_public_ip_address = true
            
            # Reference key pair resource
            key_name = aws_key_pair.ssh-key.key_name

            tags = {
                Name = "${var.env_prefix}-server"
            }
    }


    ```



   * Asign Tag

6. Deploy nginx Docker container   
7. asdfsaf

Notes:  
**Security Group**: Firewall at server level.  
**NACL (Network access list)**: Firewall at subnet level.