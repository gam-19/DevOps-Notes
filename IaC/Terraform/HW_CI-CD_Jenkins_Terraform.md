

1. Create SSH Key-pair
   AWS Mgmt Portal > EC2 > Key pair 
   Create and download new key pair 'myapp-key-pair'
2. Install TF inside Jenkins container
3. TF configuration to privision server
4. Adjust Jenkinsfile


OTHER NOTES:
* TF config files part of the app code in Source Control, which is a best practice, to include everything the application needs: the infrastructure, automation,  application configuration, app code, etc. So the CIC/CD Pipeline has access to them.
* 