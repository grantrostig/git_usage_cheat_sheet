Bring an existing (or new repo) to github via linux shell prompt:
# new repo
git init
git add .
git commit -m “first commit”
# existing – password will be prompted.
git remote add origin https://YOUR_USER_NAME_ON_GITHUB@github.com/github.com/grantrostig/NAME_OF_ALREADY_EMPTY_INTIALIZED_REPO.git
git push -u origin master
