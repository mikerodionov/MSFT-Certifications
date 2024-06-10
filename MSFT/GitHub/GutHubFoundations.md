# GitHub Foundations

## Intro to Git

- Version control definition
- Distributed version control systems, like Git
- Git vs GitHub
- Create and configure new Git project
- Make and track changes to code by using Git
- Use Git to recover from simple mistakes

### What is version control

**Version control system (VCS)** is a program or set of programs that tracks changes to a collection of files. One goal of a VCS is to easily recall earlier versions of individual files or of the entire project. Another goal is to allow several team members to work on a project, even on the same files, at the same time without affecting each other's work.

Another name for a VCS is a software configuration management (SCM) system. The two terms often are used interchangeably. Technically, version control is just one of the practices involved in SCM. A VCS can be used for projects other than software, including books and online tutorials.

### Distributed VCS

Earlier instances of VCSes, including CVS, Subversion (SVN), and Perforce, used a centralized server to store a project's history. This centralization meant that the one server was also potentially a single point of failure.

Git is distributed, which means that a project's complete history is stored both on the client and on the server. You can edit files without a network connection, check them in locally, and sync with the server when a connection becomes available. If a server goes down, you still have a local copy of the project. Technically, you don't even have to have a server. Changes could be passed around in e-mail or shared by using removable media.

### Git Terminology

- Working tree - the set of nested directories and files that contain the project that's being worked on
- Repository (repo) - the directory, located at the top level of a working tree, where Git keeps all the history and metadata for a project
- **Bare repository** -  repo that isn't part of a working tree used for sharing or backup, a bare repo is usually a directory with a name that ends in .git; in other words bare repo is a repo without working tree, just repo's metadata folder, hence no commmits can be made into it (can be created with `git init bare`)
- Hash - A number produced by a hash function that represents the contents of a file or another object as a fixed number of digits, 160 bits long, used to track file changes (takes precedence over filte time-and-date stamp)
- Object -  a Git repo contains four types of objects, each uniquely identified by an SHA-1 hash
    - blob - an ordinary file
    - tree - directory, which contains names, hashes, and permissions
    - commit - specific version of the working tree
    - tag - name attached to a commit
- Commit - action of creating commit
- Branch - a named series of linked commits, the most recent commit on a branch is called the head
- Default branch - branch created when you initialize a repository, usually called main or master in Git, the head of the current branch is named HEAD
- Remote - a named reference to another Git repository, when you create a repo, Git creates a remote named origin that is the default remote for push and pull operations
- Commands, subcommands, and options - Git operations are performed by using **commands** like `git push` and `git pull`. `git` is the command, and `push` or `pull` is the **subcommand**. The subcommand specifies the operation you want Git to perform. Commands frequently are accompanied by **options**, which use hyphens (-) or double hyphens (--). For example, `git reset --hard`.

### Git & GitHub

Git is a distributed version control system (DVCS) that multiple developers and other contributors can use to work on a project. It provides a way to work with one or more local branches and then push them to a remote repository.

GitHub is a cloud platform that uses Git as its core technology. GitHub simplifies the process of collaborating on projects and provides a website, more command-line tools, and overall flow that developers and users can use to work together. GitHub act s as the remote repository.

GitHub features:

- Issues
- Discussions
- Pull requests
- Notifications
- Labels
- Actions
- Forks
- Projects

### Git commands recap

```bash
git config --global user.name "Username"
git config --global user.email "user@donmain.com"
git config --list
mkdir testrepo
cd testrepo
git init
touch index.html
git status
git add . # or git add index.html
git commit index.html -m "Commit message"
vim index.html # modify file
git status
git commit -a -m "Commit message" # -a - add all changed files - added, modified, deleted
# ervery commit creates a version of modified file(s)
git log --oneline
vim index.html # modify file again
git diff # to see differences between the working directory and the most recent commit
code .gitignore
git status
git add -A # -A add any file that is not currently tracked
git commit -am "Commit message" # -am -  include all changes and add commit message
mkdir CSS
touch CSS/.git-keep # this file indicates to Git that this file structure has to be tracked, otherwise Git doesn't track empty folders
git add CSS
rm CSS/.git-keep
code CSS/site.css
git add .
git commit -am "To add css file" # -am -  include all changes and add commit message
git log --oneline

# Fix small error in the latest commit without creating another commit and without changing commit message
# Fixing a typo within committed file
git commit --ammend --no-edit

# Restoring deleted file - 2 scenarios
# 1 Restore deleted file from the latest commit 
rm index.html
git checkout -- index.html
# 2 Remove file from git tracking and from the working directory (with that we cannot resotore file as shown above)
git rm index.html
# To restore
git reset HEAD index.html # resets tracking of specified file to the most recent commit aka HEAD
git checkout -- index html

# Recovering after committing unwanted changes
git reset --hard HEAD^ # reset to the version right before HEAD/the latest commit 
git checkout -- CSS/site.css

# Revert to Nth prvious commit
git checkout COMMIT_HASH . # . means restore all the repo

# Other option is commit squashing - combining multiple commits into one
# Interactive rebase
git rebase -i HEAD~3
```

### Configure git

```Bash
git --version
git config --global user.name "<USER_NAME>"
git config --global user.email "<USER_EMAIL>"
git config --list
mkdir project
cd project
git init --initial-branch=main # OR git init -b main
git status
ls -a
# .git folder - stores metadata and the status of the working tree changes to keep track of what's changed in your files
git --help
```

### Basic Git commands

```Bash
git status # displays the state of the working tree & of the staging area
git add # stage changes to prepare for a commit, added but not committed changes are stored in staging area
git commit # save/snapshot staged changes
git log # shows info about previous commits
git help
git <command> --help
git help tutorial
git help tutorial-2
git help everyday
```

## Introduction to GitHub

### What is GitHub?