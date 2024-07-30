# GIT TROUBLESHOOTING


### Identifying and removing large Pushed files
   
It seems there might be an issue with the pack files in your repository. Let's try a different approach to identify large files using `git rev-list` and `git cat-file`:

1. **List All Objects with Sizes**:
    ```sh
    git rev-list --objects --all | while read -r obj path; do
        size=$(git cat-file -s "$obj")
        echo "$size $path"
    done | sort -n -r | head -n 10
    ```

This command will list the top 10 largest files in your repository. Here's a breakdown of what it does:

1. `git rev-list --objects --all`: Lists all objects in the repository along with their paths.
2. `while read -r obj path; do ... done`: Iterates over each object and path.
3. `git cat-file -s "$obj"`: Gets the size of each object.
4. `echo "$size $path"`: Prints the size and path.
5. `sort -n -r`: Sorts the output by size in descending order.
6. `head -n 10`: Takes the top 10 entries.

Run this command in your repository to identify the large files.

Now that you've identified the large file, you can proceed to remove it from the Git history. Here are the steps to do that:

1. **Remove the Large File from the Repository**:
    ```sh
    git rm --cached modules/subnet/.terraform/providers/registry.terraform.io/hashicorp/aws/5.60.0/windows_386/terraform-provider-aws_v5.60.0_x5.exe
    ```

2. **Commit the Change**:
    ```sh
    git commit -m "Remove large provider binary"
    ```

3. **Rewrite Git History to Remove the Large File**:
    ```sh
    git filter-branch --force --index-filter \
    'git rm --cached --ignore-unmatch modules/subnet/.terraform/providers/registry.terraform.io/hashicorp/aws/5.60.0/windows_386/terraform-provider-aws_v5.60.0_x5.exe' \
    --prune-empty --tag-name-filter cat -- --all
    ```

4. **Clean Up the Repository**:
    ```sh
    git reflog expire --expire=now --all && git gc --prune=now --aggressive
    ```

5. **Force Push the Changes to the Remote Repository**:
    ```sh
    git push origin --force --all
    ```

6. **Ensure `.terraform` Directory is Ignored**:
    Your `.gitignore` file already has the correct entry to ignore `.terraform` directories:
    ```gitignore
    .terraform/
    ```

By following these steps, you will remove the large file from the repository history and prevent similar issues in the future.