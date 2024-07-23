## 9. TShoot Terraform  
### Check current state of resources/infra

a) Check if there is something to create or delete.

```
  terraform plan
```

b) Query/Check detailed resources state easely  
- Check **terrafrom.tfstate**, stores the current state inside this JSON file, it has the current list of resources and their current state.  

    ```bash
        terraform state
    ```
    Has subcommands to see detailed state of terraform.  
    
    
    List all resources in the state file:
    ```bash
        terraform state list
    ```
    
    Show all attributes specific resource:
    ```bash    
        terraform state show <resource_type.resource_name>
        terraform state show aws_subnet.dev-subnet-1
    ```

#### Backup file 
**terrafrom.tfstate.backup**: Previous state applying/destroying.

