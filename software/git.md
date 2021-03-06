# Git Commands

## Latest Version Under Ubuntu
```sh
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:git-core/ppa
sudo apt-get update
sudo apt-get install git
```

## Basic commands
```sh
git log --oneline # Show log on one line
git diff --staged
cat .git/HEAD # Show where the head is
git merge --no-ff <branch_name> # Merge without Fast Forward. 
git log --oneline --graph # Show log with graph
git blame <filename>
git shortlog -sn # Show latest commiter with commit number
git shortlog --since "Jan 11 2010" -sn
```

## How to put a Bare Repository to a Remote Server
```sh
# On the server
$ cd /opt/git
$ mkdir project.git
$ cd project.git
$ git --bare init

# On the client
$ cd myproject
$ git init
$ git add .
$ git commit -m 'initial commit'
$ git remote add origin git@<git_server>:jimbeaudoin/test2.git
$ git push origin master

git clone --bare my_project my_project.git # Clone the Project into a Bare Repository
scp -r my_project.git user@server:/opt/git # Put the Bare Repository to a Server
```
## How to commit to the last commit
```sh
git add <file>
...
git commit --amend
```
## How to reset a file
```sh
git checkout -- <file> # Discard changes for a file
git reset --soft <commitID> # Moves the HEADto the <commitID>
git reset -- mixed <commitID> # 

# WARNING: REMOVE FILES!
git reset --hard # Discard all changes on all files. Going back to the last commit before the changes.

git revert <commitID> # Revert a commit and do a new commit
```
