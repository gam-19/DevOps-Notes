# JENKINS
# Jenkins > ECR > EKS


![](/Containerization/Kubernetes/_K8s-assets/jenkins_pipeline_ECS_EKS.png)  

### Pushing Docker image to ECR
1. Create ECR Repository  
    * AWS Console > Services > ECR > Create Repository > Private   
    Notes:   
        * ECR allows us unlimited number of private repos.
        * Docker Hubs only allows one prive repo.
        * We might have one repository per application.
2. Create credentials for ECR in Jenkins.  
ECR gives you instructions that you can use as reference:  
![](/AWS/_assets/ECR_push_cmds.png)  


    * Get AWS login password:  

    ```bash
        aws ecr get-login-password --region us-east-2
    ```  
    Note: 
    * It's very important that you use the '--region xxxxx' in the command above to get aws login password, otherwise, it might give you another password, and arise login failures.  
    * You already should have AWS Cli and AWS user configured.
    * Jenkins dashboard > Manage Jenkins > Credentials > System > Global credentials > New credentials  
    ID: ecr-credentials
    Kind: Username and password  
    Username: AWS
    Password: [Get it from previous point "Get AWS login password"]
3. Adjust building and tagging  

### Fetching and starting Docker image in EKS  
4. [Creating Secret for ECR](/Containerization/Creating_K8s_secret.md) credentials to be able to pull images from ECR.  
6. Edit jenkins file
* Build stage:  
    * Update credentials using creds created in Jenkins for ECR.  
    ![](/AWS/_assets/Using_DockerRepoEnvVar.png)

    * Create environmental variables for 'Docker server' and 'Docker Repo'  
    ![](/AWS/_assets/DockerRepoEnvVar.png)

7. Make sure K8s Deployment.yaml file also uses the ecr creds:
![](/AWS/_assets/K8s_Deployment_file_creds.png)
