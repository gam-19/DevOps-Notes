# TERRAFORM PROVIDERS

We use providers to connect Terrafrom to required platform or technology (AWS, Azure, Jenkins, Linode, EKS)

You can have multiple providers, best practice list them in file 'providers.tf'

![alt text](/IaC/Terraform/_terra-images/TF_providers_source.png)

'source =' tells Terraform where to download providers code, if from Hashicorp official registry or third party.