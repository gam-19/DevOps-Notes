
# Jenkins > ECR > EKS  


![](Containerization\Kubernetes\_K8s-assets\jenkins_pipeline_ECR_EKS.png)  

### Pushing Docker image to ECR
1. Create ECR Repository  
    * AWS Console > Services > ECR > Create Repository > Private   
    Notes:   
        * ECR allows us unlimited number of private repos.
        * Docker Hubs only allows one prive repo.
        * We might have one repository per application.
2. Create credentials for ECR in Jenkins.  
ECR gives you instructions that you can use as reference:  
![](AWS\Containerization\Kubernetes\_K8s-assets\ECR_push_cmds.png)  


    * Get AWS login password:  

    ```bash
        aws ecr get-login-password
    ```  
    Note: You should already have AWS Cli and AWS user configured.
    * Jenkins dashboard > Manage Jenkins > Credentials > System > Global credentials > New credentials  
    Kind: Username and password  
    Username: AWS
    Password: [Get it from previous point "Get AWS login password"]
3. Adjust building and tagging


### Fetching and starting Docker image in EKS
4. [Creating Secret for ECR](Containerization/Creating%20K8s%20secret.md) Credentials to be able to pull images from ECR.  
[create the secret](Containerization/Creating%20K8s%20secret.md)
6. Edit jenkins file
