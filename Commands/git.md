# GIT
Some useful and frequently used git commands.

### General
```git
# get the git version
git --version
# get the help
git help
```

### Setup
```git

# initialize the git repo locally
git init

# retrieve an entire repo from a hosted location
git clone <repo url>

# Check the remote repo url
git remote -v
# Adds a new remote to local git repo
git remote add origin <HTTPS or SSH URL>
# Change remote URL from HTTPS to SSH and vica-versa
git remote set-url origin <HTTPS or SSH URL>


# view all the git settings
git config --list
# list global config
git config --list --global
# view all the git settings and the origin they are coming from
git config --list --show-origin
# get the key value from the config
git config <key>
# setup the email and username (global)
git config --global user.email "<email-id>"
git config --global user.name "<username>" 
# set the default editor for git to use
git config --global core.editor <editor name>

# 
```

### Stage and commit
```git
# show modified files and files staged for commit
git status

# add/stage files for commit
git add <file or .(all)

# commit your staged content (snapshot)
git commit -m <message>

# unstage a file
git reset <file>
```

### Inspect
```git
# commit history for the current active branch
git log
# list last n number of commits
git log -n <number>
# commits between branch1 and branch2
git log <branch1>..<branch2>
# commits in branch1 that are not in branch2
git log <branch1> ^<branch2>
# lists the commits ands changes for a particular file over its history
git log -p <file>
# pretty git logs
git log --oneline --decorate --graph
```

### Branches 
```git
# Create a branch
git branch <branch-name>
# list all branches, local and remote
git branch -a
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
