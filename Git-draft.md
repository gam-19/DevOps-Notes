### The Git Cheatsheet

I have taken these from [atlassian](https://www.atlassian.com/git/tutorials/atlassian-git-cheatsheet) but writing them down and reading the description is a good way to get to know what the commands are as well as getting hands-on in everyday tasks.

### Git Basics

| Command       | Example                     | Description                                                                                                                 |
| ------------- | --------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| git init      | `git init <directory>`      | Create an empty git repository in the specified directory.                                                                  |
| git clone     | `git clone <repo>`          | Clone repository located at \<repo> onto local machine.                                                                      |
| git config    | `git config user.name`      | Define author name to be used for all commits in current repository `system`, `global`, `local` flag to set config options. |
| git add       | `git add <directory>`       | Stage all changes in \<directory> for the next commit. We can also add \<files> and \<.> for everything.                       |
| git commit -m | `git commit -m "<message>"` | Commit the staged snapshot, use \<message> to detail what is being committed.                                                |
| git status    | `git status`                | List files that are staged, unstaged and untracked.                                                                         |
| git log       | `git log`                   | Display all commit history using the default format. There are additional options with this command.                        |
| git diff      | `git diff`                  | Show unstaged changes between your index and working directory.                                                             |

### Git Undoing Changes

| Command    | Example               | Description                                                                                                                           |
| ---------- | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| git revert | `git revert <commit>` | Create a new commit that undoes all of the changes made in \<commit> then apply it to the current branch.                              |
| git reset  | `git reset <file>`    | Remove \<file> from the staging area, but leave the working directory unchanged. This unstaged a file without overwriting any changes. |
| git clean  | `git clean -n`        | Shows which files would be removed from the working directory. Use `-f` in place of `-n` to execute the clean.                        |

### Git Rewriting History

| Command    | Example              | Description                                                                                                                              |
| ---------- | -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| git commit | `git commit --amend` | Replace the last commit with the staged changes and the last commit combined. Use with nothing staged to edit the last commit’s message. |
| git rebase | `git rebase <base>`  | Rebase the current branch onto \<base>. \<base> can be a commit ID, branch name, a tag, or a relative reference to HEAD.                   |
| git reflog | `git reflog`         | Show a log of changes to the local repository’s HEAD. Add --relative-date flag to show date info or --all to show all refs.              |

### Git Branches

| Command      | Example                    | Description                                                                                                   |
| ------------ | -------------------------- | ------------------------------------------------------------------------------------------------------------- |
| git branch   | `git branch`               | List all of the branches in your repo. Add a \<branch> argument to create a new branch with the name \<branch>. |
| git checkout | `git checkout -b <branch>` | Create and check out a new branch named \<branch>. Drop the -b flag to checkout an existing branch.            |
| git merge    | `git merge <branch>`       | Merge \<branch> into the current branch.                                                                       |

### Git Remote Repositories

| Command        | Example                       | Description                                                                                                                         |
| -------------- | ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| git remote add | `git remote add <name> <url>` | Create a new connection to a remote repo. After adding a remote, you can use \<name> as a shortcut for \<url> in other commands.      |
| git remote -v      | `git remote -v` | Prints remote repos, verbose output, includes the URL for each remote repository.                                            |
| git fetch      | `git fetch <remote> <branch>` | Fetches a specific \<branch>, from the repo. Leave off \<branch> to fetch all remote refs.                                            |
| git pull       | `git pull <remote>`           | Fetch the specified remote’s copy of current branch and immediately merge it into the local copy.                                   |
| git push       | `git push <remote> <branch>`  | Push the branch to \<remote>, along with necessary commits and objects. Creates named branch in the remote repo if it doesn’t exist. |

### Git Diff

| Command           | Example             | Description                                                            |
| ----------------- | ------------------- | ---------------------------------------------------------------------- |
| git diff HEAD     | `git diff HEAD`     | Show the difference between the working directory and the last commit. |
| git diff --cached | `git diff --cached` | Show difference between staged changes and last commit                 |

### Git Config

| Command                                              | Example                                                | Description                                                                                                                                   |
| ---------------------------------------------------- | ------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------- |
| git config --global user.name \<name>                 | `git config --global user.name <name>`                 | Define the author name to be used for all commits by the current user.                                                                        |
| git config --global user.email \<email>               | `git config --global user.email <email>`               | Define author email to be used for all commits by the current user.                                                                           |
| git config --global alias \<alias-name> \<git-command> | `git config --global alias <alias-name> <git-command>` | Create shortcut for a git command .                                                                                                           |
| git config --system core.editor \<editor>             | `git config --system core.editor <editor>`             | Set the text editor to be used by commands for all users on the machine. \<editor> arg should be the comamnd that launches the desired editor. |
| git config --global --edit                           | `git config --global --edit `                          | Open the global configuration file in a text editor for manual editing.                                                                       |

### Git Rebase

| Command              | Example                | Description                                                                                                                                 |
| -------------------- | ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| git rebase -i \<base> | `git rebase -i <base>` | Interactively rebase current branch onto \<base>. Launches editor to enter commands for how each commit will be transferred to the new base. |

### Git Pull

| Command                    | Example                      | Description                                                                                                                                   |
| -------------------------- | ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| git pull --rebase \<remote> | `git pull --rebase <remote>` | Fetch the remote’s copy of current branch and rebases it into the local copy. Uses git rebase instead of the merge to integrate the branches. |

### Git Reset

| Command                   | Example                     | Description                                                                                                                                   |
| ------------------------- | --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| git reset                 | `git reset `                | Reset the staging area to match the most recent commit but leave the working directory unchanged.                                             |
| git reset --hard          | `git reset --hard`          | Reset staging area and working directory to match most recent commit and overwrites all changes in the working directory                      |
| git reset \<commit>        | `git reset <commit>`        | Move the current branch tip backwards to \<commit>, reset the staging area to match, but leave the working directory alone                     |
| git reset --hard \<commit> | `git reset --hard <commit>` | Same as previous, but resets both the staging area & working directory to match. Deletes uncommitted changes, and all commits after \<commit>. |

### Git Push

| Command                   | Example                     | Description                                                                                                                                     |
| ------------------------- | --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| git push \<remote> --force | `git push <remote> --force` | Forces the git push even if it results in a non-fast-forward merge. Do not use the --force flag unless you’re sure you know what you’re doing.  |
| git push \<remote> --all   | `git push <remote> --all`   | Push all of your local branches to the specified remote.                                                                                        |
| git push \<remote> --tags  | `git push <remote> --tags`  | Tags aren’t automatically pushed when you push a branch or use the --all flag. The --tags flag sends all of your local tags to the remote repo. |



Deleting branch  
```
#Flag -D forces deletion
git branch -D <branch_name> 

#Reflect the deletion in the remote repo
git push origin --delete <branch_name>
```

