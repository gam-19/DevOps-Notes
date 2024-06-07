## Setup authentication with Cloud Provider  
Options:
1. **Hardcode credentials**  (NOT recommended)  
In main.tf under the provider section hardcode the API **keys and secrets**. 
   
1. Set environment variables  (Highly discorraged)  
   
   E.g. AWS authentication  

   MacOS
   ```bash
    export AWS_ACCESS_KEY_ID = "[KEY]"
    export AWS_SECRET_ACCESS_KEY = "[KEY]"
    terraform apply -var-file terraform-dev.tfvars
   ```

    Windows
    ```
    Powershell
    
    $Env:AWS_ACCESS_KEY_ID = "[KEY]"
    $Env:AWS_SECRET_ACCESS_KEY = "[KEY]"

    terraform apply -var-file terraform-dev.tfvars
   ```
   NOTE: While setting credentials in environment variables works, it's **highly discouraged**.

2. **Credentials File:**
   This is highly secure alternatives to manage your Terraform credentials:

   - Create a file named `credentials` (no extension) in:
      - Linux/macOS: `~/.aws/credentials`
      - Windows: `C:\Users\<username>\.aws` (replace `<username>` with your username)
   - Set file permissions to 600 (read/write only for you):
      - Linux/macOS: `chmod 600 ~/.aws/credentials`
      - Windows: `icacls C:\Users\<username>\.aws\credentials /grant "%username%:(OI)(CI)F"`
   - Add content with your credentials, replacing bracketed values:

   ```
   [default]
   aws_access_key_id = [ACCESS_KEY_ID]
   aws_secret_access_key = [SECRET_ACCESS_KEY]
   ```

   - In your Terraform code, configure the provider to use this file:

   ```
   provider "aws" {
     region = "us-east-1"
     
     # Point to the credentials file
     shared_credentials_file = "~/.aws/credentials"
   }
   ```

3. **AWS Systems Manager Parameter Store (Recommended):**

   - Store your credentials securely within AWS. Terraform can access them securely. Refer to the AWS documentation for specific instructions.

In conclusion, prioritize security by using secure credential storage methods like a credentials file or Parameter Store. If you absolutely must use environment variables, avoid storing the actual values and acknowledge the security risks involved.
   