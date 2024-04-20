# Installing aws-iam-authenticator
### Install on Linux  
1. Download binary  

```bash
curl -o aws-iam-authenticator https://amazon-eks.s3.us-west-2.amazonaws.com/1.15.10/2020-02-22/bin/linux/amd64/aws-iam-authenticator
```
2. Give binary execution permissions  
```bash
chmod +x ./aws-iam-authenticator
```
 
3. Copy binary into your $PATH folder  
```bash
cp ./aws-iam-authenticator $HOME/bin/aws-iam-authenticator
```
   
4. PATH environment variable should contain $HOME/bin  
```bash
echo ‘export PATH=$PATH:$HOME/bin’ >> ~/.bashrc
```
   
5. Check aws-iam-authenticator is installed and working
```bash
aws-iam-authenticator help
```
   
Source:  
https://weaveworks-gitops.awsworkshop.io/60_workshop_6_ml/00_prerequisites.md/50_install_aws_iam_auth.html  
https://abhayksingh.com/how-to-install-aws-iam-authenticator-on-windows-linux-macos/
