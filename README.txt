Bring an existing (or new repo) to github via linux shell prompt:
# new repo
# cd into root of source/project tree
git init
git add .
git commit -m “first commit”
# existing – password will be prompted.
git remote add origin https://<YOUR_GITHUB_USER_NAME>@github.com/github.com/grantrostig/NAME_OF_ALREADY_INTIALIZED_EMPTY_REPO.git
git push -u origin master
