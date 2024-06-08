# TERRAFORM PROVIDERS

We use providers to connect Terrafrom to required platform or technology (AWS, Azure, Jenkins, Linode, EKS)

With Terraform we basically connect to a technology (AWS, Jenkins, Databases, etc) and use their API to create resources and configure them.

TF providers are code that "know" how to talk to those Technologies or platforms.
 
* Cloud providers expose resources for specific infrastructure platform (e.g. AWS).
* TF Providers are responsible for understanding APIs of those platforms.
* Are Plugins that Terraform uses to manage resources.

[Browse Terraform Providers](https://registry.terraform.io/browse/providers)


You can have multiple providers, best practice list them in file 'providers.tf'

![alt text](/IaC/Terraform/_terra-images/TF_providers_source.png)

'source =' tells Terraform where to download providers code, if from Hashicorp official registry or third party.