# Git Quick Guide

### 1. Setting Up Git
* Before you start using Git, configure your identity:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```
* Check your configuration:

```bash
git config --list
```


### 2. Working with a Repository
*  Clone a Repository
```bash
git clone <repository-url>
```
* Check Repository Status
```bash
git status
```
* Add Files to Staging
```bash
git add <file-name>
# Or add all changes
git add .
```
* Commit Changes
```bash
git commit -m "Your commit message"
```

### 3. Branching and Merging
* Create a New Branch
```bash
git branch <branch-name>
```
* Switch to a Branch
```bash
git checkout <branch-name>
```
* Create and Switch to a Branch
```bash
git checkout -b <branch-name>
```
* Merge Branches
```bash
git merge <branch-name>
```

### 4. Syncing with Remote
* Push Changes
```bash
git push origin <branch-name>
```
* Pull Changes
```bash
git pull origin <branch-name>
```
* Set Remote Repository
```bash
git remote add origin <repository-url>
```

### 5. Undoing Changes
* Discard Unstaged Changes
```bash
git checkout -- <file-name>
```
* Unstage Changes
```bash
git reset <file-name>
```
* Undo the Last Commit (Keep Changes)
```bash
git reset --soft HEAD~1
```
* Undo the Last Commit (Discard Changes)
```bash
git reset --hard HEAD~1
```

### 6. Viewing Logs and History
* Show Commit History
```bash
git log
```
* Show Changes in Commit History
```bash
git log -p
```
* Show Graphical History
```bash
git log --oneline --graph --decorate --all
```

# Git Cheatsheet
```
| Command                              | Description                              |
|--------------------------------------|------------------------------------------|
| `git config --global`                | Configure global username/email          |
| `git init`                           | Initialize a repository                  |
| `git clone <url>`                    | Clone a repository                       |
| `git status`                         | Show the working directory status        |
| `git add <file>` / `git add .`       | Add file(s) to the staging area          |
| `git commit -m "<message>"`          | Commit staged changes                    |
| `git branch`                         | List branches                            |
| `git branch <branch-name>`           | Create a new branch                      |
| `git checkout <branch-name>`         | Switch to a branch                       |
| `git checkout -b <branch-name>`      | Create and switch to a branch            |
| `git merge <branch-name>`            | Merge a branch into the current branch   |
| `git pull origin <branch-name>`      | Pull updates from a remote branch        |
| `git push origin <branch-name>`      | Push changes to a remote branch          |
| `git reset <file>`                   | Unstage a file                           |
| `git reset --soft HEAD~1`            | Undo last commit but keep changes        |
| `git reset --hard HEAD~1`            | Undo last commit and discard changes     |
| `git log`                            | Show commit history                      |
| `git log --oneline --graph --all`    | Show graphical commit history            |
```
--- 
For more advanced Git commands, consult the [Git documentation](https://git-scm.com/doc).