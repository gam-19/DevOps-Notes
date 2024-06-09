1. Create custom **VPC**
2. Create custom **Subnet**  
    In one AZ
3. Connect VPC to Internet  
   Allow traffic in/out
   * Create **Route Table**  
   It's like a virtual Router in the VPC. Will forward traffic of the vpc.
   * Create **Internet Gateway**
4. Provision **EC2 Instance**
5. Deploy nginx Docker container
6. Create **Security Group** (Firewall)
   * Allow access to web server from Internet.
   * Allow SSH connecting for mgmt.
7. asdfsaf

Notes:  
Security Group: Firewall at server level.
NACL (Network access list): Firewall at subnet level.