# notes on notation: replace <WHAT_EVER> with a value to fits your situation. [] enclosed stuff is optional.  \ means also type the next line on the original line

#========== Bring/export/upload an existing (or new) git repository/repo to github.com website via linux shell prompt:
#=== if new repo
# $cd into root of source/project tree
git init
git add .                          # "." adds everything, if that is what you want, else <FILE_NAME>
git commit -m “<my first commit>”  # OR:-am “<my first commit>”  # add all & commit.
#=== if existing already initialized but empty repo on github
git remote add origin \
https://<YOUR_GITHUB_USERNAME>@github.com/<YOUR_GITHUB_USERNAME>/<NAME_OF_ALREADY_INTIALIZED_EMPTY_REPO>.git
git remote -v                   # optional step: go ahead verify it, if you want
git push -u origin master       # --set-upstream # add upstream (tracking) reference

#========== Clone / Pull down / download / Bring an "original repo" to your system
git clone <REPO> #  append if there are submodules you want: --recursive
git submodule update --init --recursive  # in case there were git submodules you didn't get with a normal clone

#========== Stash some code and then later Stash Pop (ie. un-stash) it.
git stash push [-m <msg>]    # saves away your current work from 'workspace' to the 'stash'
git stash list
git stash show [<stash>]    # where <stash> is probably an integer like '0'.
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
git branch -lavv   #OR: -r will singly list the remotes
git status -vv 
git show --pretty=fuller [<object>]  
git log [< >..< >] [<text>]
git log -- <FILE_NAME> #
git log -p         # also show changes.
git diff           # diff between workspace and index.
git diff --staged  # diff between index and local repo, what we are about to commit.
git fsck           # verify the integrity of the files in the entire repo.
git show
git show-ref --heads
git diff HEAD  # shows what "commit -a" would do  # use "git gui" instead.

#========== Rebase (the alternative to merge - an area of disagreement between many)
# you are told to rebase to the current state of master by the technical lead, so you perform these steps:
# first get your local repo into a stable state, meaning what?: fetch??, commit??, stash??
git rebase origin/master

https://help.github.com/en/articles/addressing-merge-conflicts

#========== Pull changes for "your clone" you made of another "origianal repo"
# https://help.github.com/en/articles/merging-an-upstream-repository-into-your-fork
git checkout master
git pull https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git master
git push origin master  # push to your github repo if you are ready

#========== Config
git config --list
# --global OR --local
git config --global user.name  "<YOUR NAME>"
git config --global user.email "<YOUR FULL EMAIL ADDR>"

#========== Dangerous Stuff
git rm <FILE_NAME>          # kills file in the workspace (ie. the filesystem), ALSO stages removal from repo.  If file had been previously committed, it can be brought back from history.
git rm --cached <FILE_NAME> # kills file in the workspace (ie. the filesystem), ALSO stages removal from repo.
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
man git-push  # yes, git has man pages on linux!
git help push
#========== Good/complete info on software licenses to attach to your new repo.
https://choosealicense.com/appendix/
