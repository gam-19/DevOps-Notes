User data = execute commands after the resource like EC2 instance is created.

1. Option 1: write the code inside tf file.  

![](/IaC/Terraform/_terra-images/user-data-in-tf-file.png)

2. Option 2: Have separated "entry script" e.g: entry-script.sh
![](/IaC/Terraform/_terra-images/user-data-in-sh-file.png)
3. Each Cloud Provide has their on way.
4. Use Terraform "provisioner" 

![](/IaC/Terraform/_terra-images/tf-provisiones-n-connection.png)

Note:   
* Terraforms does not recommends to use provisioners, should be used as last resort.
* Terraform recommend as best practice to use Configuration Management tools, or CD/CD tool, once server is provioned hand it over to those tools.
* The only provisioner recommended is "local-exec" to handle local files, TF will manage the current status, changes, etc.