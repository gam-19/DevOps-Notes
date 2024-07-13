# Namespaces

## What are Namespaces?


* Allow to group resources within a single cluster.
* Like a virtual cluster inside a cluster.

### Other notes
* Names of resources need to be unique within a namespace, but not across namespaces

* For cluster more than 10 user, altough some recomend to use it always.  
  
#### Namespaces cmds:

```bash
  #Show resources that are not part of any namespace
  kubectl api-resources --namespaced=false

  #Show resources in namespaces
  kubectl api-resources --namespaced=true

```


#### Creating namespace:

Via kubectl command:
```bash
kubectl create namespace <name>
kubectl create namespace dev
```

#### Changing default / "active" namespace:
```bash
kubectl config set-context --current --namespace=<my-namespace>
```
Subsequent commands will use that context (namespace)
Alternatevely, you can use 'kubens' tool:
```bash
    # Install tool
    brew install kubectx

    # List namespaces
    kubens

    # Change current namespace
    kubens <my-namespace>

```

Via configuration file:  

![alt text](/Containerization/Kubernetes/_K8s-assets/Create_namespace_conf_file.png)


What are Namespaces?
Why we use them?
## Use cases
1.  Group resources logically
    * Instead of having all in the "default" namespace.
2.  Isolate team resources
    * To avoid conflicts
3.  Share resources between different
4.  Limit permissions and compute resources for teams.
5.  

C
