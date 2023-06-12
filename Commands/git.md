# GIT
Some useful and frequently used git commands.

### Config
```git
# list global config
git config --list --global

# setup the email and username (global)
git config --global user.email "<email-id>"
git config --global user.name "<username>" 
```

### Branches 
```git

# Create a branch
git branch <branch-name>

# switch to a existing branch
git checkout <branch-name>

# create and switch
git checkout -b <branch-name>

# delete a branch
git branch --delete <branch-name>

# push main branch to remote
git push origin main

# push a new local branch to remote
git push -u origin <branch-name>

# push all branches to remote
git push --all origin
```