# note on notation: replace <WHAT_EVER> with a value to fits your situation.

#========== Bring/export/upload an existing (or new) git repository/repo to github.com website via linux shell prompt:
#=== if new repo
# $cd into root of source/project tree
git init
git add .                       # "." adds everything, if that is what you want, else <FILE_NAME>
git commit -m “<my first commit>”
git commit -am “<my first commit>”  # add and commit at once.
#=== if existing already initialized but empty repo on github
git remote add origin https://<YOUR_GITHUB_USER_NAME>@github.com/github.com/grantrostig/<NAME_OF_ALREADY_INTIALIZED_EMPTY_REPO>.git
git remote -v                   # optional step: go ahead verify it, if you want
git push -u origin master

#========== Pull down / download / Bring to your system a repo TODO....

#========== Good/complete info on software licenses
https://choosealicense.com/appendix/

#========== Stash some code and then later Stash Pop (ie. un-stash) it.
git stash [save] [<msg>]    # saves away your current work from 'workspace' to the 'stash'
git stash list
git stash show [<stash>]    # where stash is probably an integer like '0'.
git stash pop               # applies latest stashed changes and removes it from stash     
# git stash apply [<stash>] # same as above but doesn't remove the stashed changes from the stash

#========== Figure out what is happing!?!
# summary of the 5 unique states a change (to a file) can be in:
# stash,
# workspace (CONFUSION FACTOR: working tree/directory refers to both workspace and index)),
# index     (AKA staging area, staged files, current directory cache) ("check-in changes to index" AKA "stage changes"),
# local repository,
# upstream (or origin?) repository
git stash list
git fetch -v
git branch -lavv
git status -vv 
git show --pretty=fuller [<object>]  
git log
git log -- <FILE_NAME> #
git log -p         # also show changes.
git diff           # diff between workspace and index.
git diff --staged  # diff between index and local repo, what we are about to commit.

#========== Rebase (the alternative to merge - an area of disagreement between many)
# you are told to rebase to the current state of master by the technical lead, so you perform these steps:
# first get your local repo into a stable state, meaning what?: fetch??, commit??, stash??
git rebase origin/master

#========== Config
git config --list
# --global OR --local
git config --global user.name  "<YOUR NAME>"
git config --global user.email "<YOUR FULL EMAIL ADDR>"

#========== Dangerous Stuff
git rm <FILE_NAME>          # kills file in the workspace (ie. the filesystem), ALSO stages removal from repo.  If file had been previously committed, it can be brought back from history.
git checkout -- <FILE_NAME> # throws away any workspace (never-indexed/never-staged) changes to file PERMANENTLY.
git checkout <CHANGE_ID-BEFORE_DELETE> -- <FILE_NAME> # restores a file into workspace from repo that was previously deleted and delete was commited, ALSO stages whole file to index.
git reset HEAD <FILE_NAME>  # throws away any indexed/staged changes to file, but changes are still in workspace.

#=============== QtCreator IDE faciliated commands =====
# amend commit
git commit -F C:\Users\grostig\AppData\Local\Temp\QtCreator.ZpdSkp --amend
# remove file, from the Qt project and index, but probably not from the file system. todo:??
git rm --force git_usage_cheat_sheet.files

#=============== High Quality Reference/Tutorial Resources for GIT =====
http://www.ndpsoftware.com/git-cheatsheet.html
https://www.youtube.com/watch?v=uR6G2v_WsRA  # David Mahler series of 3 videos.
https://www.youtube.com/watch?v=FyAAIHHClqI
https://www.youtube.com/watch?v=Gg4bLk8cGNo

