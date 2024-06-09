
Gral procedure
1. Create local folder and initialize local Git repo
   ```
    git init
   ```
2. Connect local git to remote repo
   ```bash
   git remote add origin https://gitlab.com/gam-19/terraform-learn-2.git
   ```
3. Rename or make sure main (master) branch is named 'main':
   ```bash
   git branch -M main
   ```
   -M flag: moves renames.

4. Possible issues:
   You might get this error:

   *PS C:\terraform2> git push origin main 
   [rejected] main -> main (non-fast-forward) error: failed to push some refs to.... hint: Updates were rejected because the tip of your current branch is behind hint: its remote counterpart
  
This usually happens when you initialize a new Git repository locally and add a remote that already has commits in it, or when the history of the remote repository is rewritten.

To resolve this issue, you can use the `--allow-unrelated-histories` option with the `git pull` command. This option tells Git to merge the two sets of changes even if they have unrelated histories.

```bash
git pull --allow-unrelated-histories origin main
```

After running this command, you might have to resolve any merge conflicts if there are conflicting changes in the two repositories. Once you've resolved the conflicts and committed the changes, you should be able to push your changes to the remote repository with the `git push origin main` command.