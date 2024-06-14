* EC2 > Key pairs > Create key pair
* Key pair type: RSA
* Private key file format: .pem
* Download to user C:\Users\gam\.ssh or equivalent in Linux.
* You must Restrict permissions on .pem file.  

```
Powershell

    #List permissions
    Get-Acl -Path .\server-key-pair.pem | Format-List
    
    # Remove inherited permissions
    icacls .\server-key-pair.pem /inheritance:r

    # Assign read permission to current user
    icacls .\server-key-pair.pem /grant:r "$($env:USERNAME):(R)"


  ```
* SSH server:
  ```bash

  ```
