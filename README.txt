GIT command usage examples and cheat sheet && other good stuff.
# notes on notation: replace <WHAT_EVER> with a value to fits your situation. [] enclosed stuff is optional.  
#   ... \ means also type the next line on the original line, meaning we have a long line that has wrapped to the next line in this document.

#========= Bring/export/upload an existing (or new) git repository/repo which is on your local computer to the github.com website via linux shell prompt:
cd into root of source/project tree
#=== New git repo to be created within an empty or non-empty project directory on local computer
git init
git add .                          # "." adds everything, if that is what you want, else <FILE_NAME>
git commit -m “<my first commit>”  # OR:-am “<my first commit>”  # add all & commit.
#=== Connecting and pushing code to existing already created, but empty repo, on github, 
#=== if using SSH:
  git remote add origin git@bitbucket.org:<YOUR_GITHUB_USERNAME>/<NAME_OF_ALREADY_INTIALIZED_EMPTY_REPO>.git
                 OR ^^  git@github.com
#=== else if using HTTPS:
  git remote add origin \
      https://<YOUR_GITHUB_USERNAME>@github.com/<YOUR_GITHUB_USERNAME>/<NAME_OF_ALREADY_INTIALIZED_EMPTY_REPO>.git
#=== end if               
git remote -v                   # optional step: go ahead verify it, if you want
git branch -M master            # github claims it wants "main" but that is just PC and wrong
git push -u origin master       # --set-upstream # add upstream (tracking) reference
#=== Done :)

#========== Clone/Pull down/download/ Bring an "original repo" to your system
git clone <REPO> # <REPO> is either HTTPS or SSH *.git URLS as above, 
# also: if there are submodules you want, append this to above: --recursive
git submodule update --init --recursive  # in case there were git submodules you didn't get using a normal clone (ie. you forgot to use --recursive)

#========== When SSH gives an error - 
# Note your key files in ~/.ssh must not be open to reading by others or you will 
# get a generic "invalid key" error, because ssh will silently ignore them.
# fedora33 error on clone: sign_and_send_pubkey: signing failed for RSA "/home/grostig/.ssh/id_rsa" from agent: 
#   ... agent refused operation \n git@github.com: Permission denied (publickey).
stat -c '%a %n' ~/.ssh ~/.ssh/*
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ecdsa
chmod 600 ~/.ssh/id_ed25519
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub # (and others from above)
ssh -vT git@github.com   # output gives you a hint, tells if authentication worked
# try this https://help.github.com/en/articles/error-permission-denied-publickey

#========== Stash some code and then later Stash Pop (ie. un-stash) it.
git stash push [-m <msg>]    # saves away your current work from 'workspace' to the 'stash'
git stash list
git stash show [<stash>]    # where <stash> is probably an integer like '0'.
git stash pop               # applies latest stashed changes and removes it from stash     
# git stash apply [<stash>] # same as above but doesn't remove the stashed changes from the stash

#========== Figure out what is happing!?!
# summary of the 5 unique states a change (to a file) can be in:
# - stash,
# - workspace (CONFUSION FACTOR: working tree/directory refers to both workspace and index)),
# - index     (AKA staging area, staged files, current directory cache) ("check-in changes to index" AKA "stage changes"),
# - local repository,
# - upstream (or origin?) repository
more ./.git/config # gives $ remote "origin" url = git@github.com:grantrostig/file_maintenance_clipped.git $ fetch = +refs/heads/*:refs/remotes/origin/*
git fetch -v  # gives: $ From github.com:grantrostig/file_maintenance_clipped $ =[up to date] master->origin/master
git stash list
git branch -lavv   #OR: -r will singly list the remotes
git status -vv -uall --find-renames --renames 
git show --pretty=fuller --notes [<object>...]  
git log [< >..< >] [<text>] --pretty=fuller
git log -- <FILE_NAME> #
git log -p         # also show changes.  -p is not documented anymore??
git diff           # diff between workspace and index.
git diff --staged  # diff between index and local repo, what we are about to commit.
git fsck --unreachable --cache --root --tags --strict          # verify the integrity of the files in the entire repo.
git show-ref --head --verify
git diff --cached  # shows what "commit" would do # use "git gui" instead.
git diff HEAD      # shows what "commit -a" would do
gh repo list worthy-os --limit 1 --json name,diskUsage # yields:"diskUsage": 254978
 { probably 1k blocks, about 308MB on my linux disk below is what clone said: 
  remote: Enumerating objects: 274949, done.
  remote: Counting objects: 100% (152/152), done.
  remote: Compressing objects: 100% (132/132), done.
  remote: Total 274949 (delta 87), reused 49 (delta 20), pack-reused 274797
  Receiving objects: 100% (274949/274949), 249.05 MiB | 168.00 KiB/s, done. }

Some potential background on size:
  https://stackoverflow.com/questions/8646517/how-can-i-see-the-size-of-a-github-repository-before-cloning-it
  https://gist.github.com/magnetikonline/dd5837d597722c9c2d5dfa16d8efe5b9
  https://stackoverflow.com/questions/72170296/how-to-get-the-size-of-my-organization-repositories
  OR Simply type: https://api.github.com/repos/orgname_ifany/repo_name on the browser
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
git config -l
git config --global user.name  "<YOUR NAME>"
git config --global user.email "<YOUR FULL EMAIL ADDR>"
# --global OR --local OR --system OR --worktree OR -f OR --blob <block-id>

#========== Dangerous Stuff
git rm <FILE_NAME>          # kills file in the workspace (ie. the filesystem), ALSO stages removal from repo. 
#   ... If file had been previously committed, it can be brought back from history.
git rm --cached <FILE_NAME> # kills file in the workspace (ie. the filesystem), ALSO stages removal from repo.
git checkout -- <FILE_NAME> # throws away any workspace (never-indexed/never-staged) changes to file PERMANENTLY.
git checkout <CHANGE_ID-BEFORE_DELETE> -- <FILE_NAME> # restores a file into workspace from repo that was previously 
#   ... deleted and delete was commited, ALSO stages whole file to index.
git reset HEAD <FILE_NAME>  # throws away any indexed/staged changes to file, but changes are still in workspace.
#========== QtCreator IDE faciliated GIT commands =====
# amend commit
git commit -F C:\Users\grostig\AppData\Local\Temp\QtCreator.ZpdSkp --amend
# remove file, from the Qt project and index, but probably not from the file system. todo:??
git rm --force git_usage_cheat_sheet.files
#========== Linux git tools I use that work together as a unit. Not sure if latter two are automatically installed with first.
git cola
git DAG
gitk
#========== Start over without rm -r and clone again
$ # get local to initial state, done by GitCola
$ git push origin master --force # to push it back to messed up repo on github.com
#========== High Quality Reference/Tutorial Resources for GIT =====
http://www.ndpsoftware.com/git-cheatsheet.html
https://www.youtube.com/watch?v=uR6G2v_WsRA  # David Mahler series of 3 videos.
https://www.youtube.com/watch?v=FyAAIHHClqI
https://www.youtube.com/watch?v=Gg4bLk8cGNo
man git-push  # tells about "git push ..."
git help push # same as above line
#==================== Other stuff
#========== Good/complete info on software licenses to attach to your new repo.
https://choosealicense.com/appendix/
#========== Links-Symbolic: create
$ symlink -s relative -r interactive_clobber -i
$ ln -svri Makefile ../lib_tty/Makefile
#========== finding files and or text file content
$ tree -af | grep <FILENAME-PARTIAL>
$ rg -n -w '[A-Z]+_SUSPEND' # ripgrep (Unicode)
$ find . -iname <FILENAME>  # gives errors for directories where no permissions
$ find . -type d -regex ".*/src/.*" -exec grep -r "io_context" {} \;
$ grep -Rnwe <FILE-CONTENT-SEARCH-STRING> # add l for only the file name
$ grep -rE "^.*asio.*async.*|io_context" --include "*.hpp" --include "*.cpp" .
$ rpm -ql <PACKAGE_NAME> # lists files within rpm package.
$ sudo dnf whatprovides '*libbacktrace*'
#========== Network Diagnois
$ sudo nethogs
$ ping -D -O -W 120 -i 60 google.com | tee -a Ranch_Wireless_Intermittent_Problem.txt 
#========== Cmake compile
$ cd elements
$ mkdir build
$ cd build
$ cmake ../ -G "Unix Makefiles" -DELEMENTS_HOST_UI_LIBRARY=gtk
