# TERRAFORM PROVIDERS

* We use providers to connect Terrafrom to required platform or technology (AWS, Azure, Jenkins, Linode, EKS)
* Basically connect to their API to create resources and configure them.
* Providers are code that "know" how to talk to those Technologies or platforms. 
* Cloud providers expose resources for specific infrastructure platform (e.g. AWS).
* Are Plugins that Terraform uses to manage resources.
*  Providers are ownend and maintained by Hashicorp, but also there are third party technology providers, and community Partner providers that create them and actively maintain them.

[Browse Terraform Providers](https://registry.terraform.io/browse/providers)


* You can have multiple providers, best practice list them in file 'providers.tf', and keep code clean.

![alt text](/IaC/Terraform/_terra-images/TF_providers_source.png)

'source =' tells Terraform where to download providers code, if from Hashicorp official registry or third party.