# GitHub Foundations

## Intro to Git

- Version control definition
- Distributed version control systems, like Git
- Git vs GitHub
- Create and configure new Git project
- Make and track changes to code by using Git
- Use Git to recover from simple mistakes

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
```
