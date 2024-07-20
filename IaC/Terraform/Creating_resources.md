## CREATING RESOURCES WITH TERRAFORM

Prerequisites:
- Install Terraform
- Setup TF locally
- Connect to AWS account
 
Procedure:
1. Create a project folder / work directory
2. Start creating Terraform config files . Ej. main.tf, use can use VS Code.   
   
   Optionally: you can add Terraform plugins (help to Syntax highlighting, auto-completion, etc) like 'Terraform' by Anton Kulikov or Hashicorp itself.  
3. Use provider
   You can refer TF docs to see syntax.
     * Name
     * Region
     * (Access keys)
     * Define source and provider version.  
        Note: For official providers no need to set source, but for third party it is.  

        ![alt text](/IaC/Terraform/_terra-images/TF_providers_source.png)

4. Start creating resources
    * 'resource' to create resource.
    * 'data' to query existing resource.