# KUBECTL  

## USEFUL COMMANDS

### Kubectl cluster location 
**Point** Kubectl to **another cluster** location. E.g from AWS EKS to Linode LKE.
```bash
    export KUBECONFIG=~/Downloads/test-kubeconfig.yaml
```

KUBECONFIG is an environmental variable,  sets the context of which cluster needs to connect kubectl, includes cluster credentials.  
