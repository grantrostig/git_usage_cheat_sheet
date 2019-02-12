# *** Bring/export/upload an existing (or new) git repository/repo to github.com website via linux shell prompt:
# note on notation: replace <WHAT_EVER> with a value to fits your situation.
# new repo
# cd into root of source/project tree
git init
git add .                       # adds everything, if that is what you want.  
git commit -m “<my first commit>”
# existing – password will be prompted.
git remote add origin https://<YOUR_GITHUB_USER_NAME>@github.com/github.com/grantrostig/<NAME_OF_ALREADY_INTIALIZED_EMPTY_REPO>.git
git remote -v                   # verify it if you want
git push -u origin master

# *** Pull down / download / Bring to your system a repo TODO....

