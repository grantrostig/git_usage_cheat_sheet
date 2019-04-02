# ========== Bring/export/upload an existing (or new) git repository/repo to github.com website via linux shell prompt:
# note on notation: replace <WHAT_EVER> with a value to fits your situation.

# === if new repo
# cd into root of source/project tree
git init
git add .                       # adds everything, if that is what you want.  
git commit -m “<my first commit>”

# === if existing already initialized but empty repo on github
git remote add origin https://<YOUR_GITHUB_USER_NAME>@github.com/github.com/grantrostig/<NAME_OF_ALREADY_INTIALIZED_EMPTY_REPO>.git
git remote -v                   # optional step: go ahead verify it, if you want
git push -u origin master

# ========== Pull down / download / Bring to your system a repo TODO....

# ========== Good/complete info on software licenses
https://choosealicense.com/appendix/

# ========== Stash some code and then later Stash Pop (ie. un-stash) it.
git stash [save] [<msg>]    # saves away your current work from 'workspace' to the 'stash'
git stash list
git stash show [<stash>]    # where stash is probably an integer like '0'.
git stash pop               # applies latest stashed changes and removes it from stash     
# git stash apply [<stash>] # same as above but doesn't remove the stashed changes from the stash

# ========== Figure out what is happing!?!
# summary of the 5 possible singular states a change (to a file) can be in:
# stash, workspace, index (AKA staging,cache), local repository, upstream (or origin?) repository
git stash list
git fetch -v
git branch -lavv
git status -vv 
git show --pretty=fuller [<object>]  
git log ....?
git diff ....?

# ========== Rebase (the alternative to merge - an area of disagreement between many)
# you are told to rebase to the current state of master by the technical lead, so you perform these steps:
# first get your local repo into a stable state, meaning what?: fetch??, commit??, stash??
git rebase origin/master



