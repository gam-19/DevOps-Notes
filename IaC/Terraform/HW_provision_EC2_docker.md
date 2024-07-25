Oveview, steps:
Create custom VPC
Create custom Subnet
Create Route Table & Internet Gateway
Provision EC2 Instance
Deploy nginx Docker container
Create Security Group (Firewall)  






1. Create custom **VPC**  
2. Create custom **Subnet**    
    In one AZ
 ```HCL
 # main.tf

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
 ```
 #terraform.tfvars
    vpc_cidr_block = "10.0.0.0/16"
    subnet_cidr_block = "10.0.10.0/24"
    avail_zone = "us-east-2b"
    env_prefix = "dev"
 ```
3. Connect VPC to Internet  
   Using custom Route table to allow traffic in/out
   * Create **Route Table**  
   It's like a virtual Router in the VPC. Will forward traffic of the vpc.
   * Create **Internet Gateway**
   * Associate subnet to internet gateway.

    ```HCL
    # Creating custom route table
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
    Using default Route table instead  
    ```BASH
    HCL - Terraform
    # creating custom internet gateway
    resource "aws_internet_gateway" "myapp-igw" {
        vpc_id = aws_vpc.myapp-vpc.id
        tags = {
            Name: "${var.env_prefix}-igw"
        }
    }

    # Using default route table
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
    # Subnet association to route table happens automatically, no need to specify.    
    ```

4. Create **Security Group** (Firewall)
   * Allow web access from Internet.
   * Allow SSH connecting for mgmt.

    Using custom segurity group
```BASH
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
```

Using existing default segurity group
        
```BASH
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
   * Associate Public IP
   * Create access keys
     * a) Create access key on aws and associate key name with EC2 instance. You also have to download the private key .pem to yoour computer so you can ssh the EC2 instance.
     * b) Or generate private and pub keys locally, point pub key for automated key pair creation.
   * Install, run Docker container script.

    ```bash
    # In main.tf
        
        # Variables
        variable "vpc_cidr_block" {}
        variable "subnet_cidr_block" {}
        variable "avail_zone" {}
        variable "env_prefix" {}
        variable "my_ip" {}
        variable "instance_type" {}
        variable "public_key_location" {}
        
        # Query latest Amazon Linux image
        data "aws_ami" "latest-amazon-linux-image" {
            most_recent = true
            owners = ["amazon"]
            filter {
                name = "name"
                values = ["amzn2-ami-kernel-*-x86_64-gp2"]
            }
            filter {
                name = "virtualization-type"
                values = ["hvm"]
            }
        }
        
        # Create key pair resource
        resource "aws_key_pair" "ssh-key" {
            key_name = "server-key"
            # reference to public key file path
            public_key = file(var.public_key_location)
        }
        
        resource "aws_instance" "myapp-server" {
            ami = data.aws_ami.latest-amazon-linux-image.id
            instance_type = var.instance_type
            
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

    # In terraform.tfvars file
        # Path to public key created in advance.
        public_key_location = "C:\\Users\\gam\\.ssh\\id_rsa.pub"
    ```
    * Asign Tag

6. Deploy nginx Docker container  
   Option 1
    ```bash
        resource "aws_instance" "myapp-server" {            
            # Using 'user_data' inside instance code, aws provider attribute to run script after EC2 creation  
            user_data = <<EOF
                            #!/bin/bash
                            sudo yum update -y && sudo yum install docker -y
                            sudo systemctl start docker
                            sudo usermod -aG docker ec2-user
                            docker run -p 8080:80 nginx
                        EOF
            #Run again the script only if there is change of the script itself.
            user_data_replace_on_change = true
            
            tags = {
                Name = "${var.env_prefix}-server"
            }
        }
    ```
    Option 2:
    ```bash
        resource "aws_instance" "myapp-server" {
            
            # Using 'user_data' to reference a script file
            user_data = file("entry-script.sh")
            # Create a script file with that name in terraform directory, and place script commands inside
            user_data_replace_on_change = true
            tags = {
                Name = "${var.env_prefix}-server"
            }
        }

            #'entry-script.sh' file content:
            #!/bin/bash
            sudo yum update -y && sudo yum install docker -y
            sudo systemctl start docker
            sudo usermod -aG docker ec2-user
            docker run -p 8080:80 nginx
    ```
    Note: Op 1 and 2 script commands above are passed to AWS to execute them on virtual server.

    Option 3: 
    'remote-exec' connects via ssh using Terraform, and TF itself run cmds in virtual servers.
    Provisioners are last resort, not recommended by Terraform.
    
    ```bash
    resource "aws_instance" "myapp-server" {
        # other code...
                
        user_data_replace_on_change = true
        
        # Connection for 'remote-exec'
        connection {
            type = "ssh"
            host = self.public_ip
            user = "ec2-user"
            private_key = file(var.private_key_location)
        }

        # Provisioner to copy script file to aws instance
        provisioner "file" {
            source = "entry-script.sh"
            destination = "/home/ec2-user/entry-script.sh"
        }

        # Provisioner to execute script, file should already exist in ec2 instance
        provisioner "remote-exec" {
            inline = ["/home/ec2-user/entry-script.sh"]
        }

        tags = {
            Name: "${var.env_prefix}-server"
        }
    }


    ```


Notes:  
**Security Group**: Firewall at server level.  
**NACL (Network access list)**: Firewall at subnet level.