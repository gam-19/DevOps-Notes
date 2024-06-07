In TerraformWhat what are Tags used for?
* For human consumption, so we can have more information about the resource usage.
* For referencing resources components from other components (programatically).  
  E.g. One of the process of the EKS Control Plane is the 'K8s Cloud Control Manager' and needs to know what VPC, what subnets are for specific EKS cluster, since we might have multiples vpc, so Tags help to reference what components belong to what EKS clusters.
  Also ELB use those tags.