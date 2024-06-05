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
mkdir CSS
touch CSS/.git-keep # this file indicates to Git that this file structure has to be tracked, otherwise Git doesn't track empty folders
git add CSS
rm CSS/.git-keep
code CSS/site.css
git add .
git commit -am "To add css file"
git log --oneline

# Edit small error in the latest commit without creating another commit and without changing commit message
# Fixin a typo within committed file
git commit --ammend --no-edit
# Restore deleted file from the latest commit 
rm index.html
git checkout -- index.html
# Remove file from git tracking and from the working directory (withat we cannot resotoree file as shown above)
git rm index.html
# To restore
git reset HEAD index.html # resets tracking of specified file to the most recent commit aka HEAD
git checkout -- index html
# Recovering after committing unwanted changes
git reset --hard HEAD^ # reset to the version right before HEAD/the latest commit 
git checkout -- CSS/site.css
# Revert to Nth prvious commit
git revert --no-edit HEAD # undo the latest commit changes, creates a new commit which undoes the latest one / revert to the most recent commit
# to rollback to specifi and not the latest commit
git checkout COMMIT_HASH . # . means restore all the repo
# Other option is commit squashing - combining multiple commits into one
```
