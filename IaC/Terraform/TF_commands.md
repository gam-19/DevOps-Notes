## 8. TF Core commands  
- `terraform init` = get your project folder ready with providers
- `terraform plan` = show what is going to be created, and changed during the next command based on our code.
Also shows provider's list of **available resources names**.  

### Deploying resources
Deploy resources defined in our code.
```bash
  terraform apply
```

Deploy configuration file without asking for confirmation.
```bash
  terraform apply -auto-approve
```

Deploy config file using specific variable files.
```bash
  terraform apply -var-fle <terraform-dev.tfvars>
```


### Deleting resources
Option 1: Just remove resource from config file.  
And 'terraform apply' again. This way your config file will match current state of your infra. Best practice specially if working in team.

Option 2: Delete specific resource
```
  terraform destroy -target aws_subnet.dev-subnet-2
```

Option 3:
```
  terraform destroy
```
Will destroy all  resources in our current config file.  
Note: Good thing, you don't have to know in which **order** you need **to delete the resources**.