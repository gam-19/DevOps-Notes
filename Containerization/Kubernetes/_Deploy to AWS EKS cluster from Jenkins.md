#### Intall Kubectl  
Inside same host where Jenkins is running (container, vm or host).

#### Install [aws-iam-authenticator](AWS\aws-iam-authenticator.md)
Inside Jenkins Container

#### Create **kubeconfig file** to connect to EKS cluster
* Created **./kube/config** and copied inside the Jenkins Container  
Tip: Inside 'jenkis' user.

#### Created Jenkins Credential 
#### Create/update **Jenkinsfile** that deploys to EKS cluster



#### Installing / Updating Kubectl

Sources:
https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html  
https://www.geeksforgeeks.org/install-and-set-up-kubectl-on-linux/  
https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/  
