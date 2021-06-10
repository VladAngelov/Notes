# GIT commands

--- | TELL GIT WHO YOU ARE | ---
--- | -------------------- | ---
Configure the author name and email address to be used with your commits: | --- | git config --global user.name "Sam Smith"
--- | --- | git config --global user.email sam@example.com
  |   |   
--- | CREATE A NEW LOCAL REPOSITORY | ---
Create a new local repository: | --- | git init
  |   |   
--- | CHECK OUT A REPOSITORY | ---
Create a working copy of a local repository: | --- | git clone /path/to/repository
For a remote server, use: | --- | git clone username@host:/path/to/repository
  |   |   
--- | ADD FILES | ---
Add one: | --- | git add <filename>
More files to staging (index): | --- | git add *
Add all files: | --- | git add .
  |   |   
--- | COMMIT | ---
Commit changes to head (but not yet to the remote repository): | --- | 	git commit -m "Commit message"
Commit any files you've added with git add, and also commit any files you've changed since then: | --- | git commit -a
Add all files: | --- | git add .
  |   |  
--- | PUSH | ---
Send changes to the master branch of your remote repository: | --- | git push origin master
  |   |  
--- | STATUS | --- 
List the files you've changed and those you still need to add or commit: | --- | git status	
  |   |  
--- | CONNECT TO A REMOTE REPOSITORY | ---
Add the server to be able to push to it: | --- | git remote add origin <server>
List all currently configured remote repositories: | --- | git remote -v 
  |   |  
--- | BRANCHES | ---
Create a new branch and switch to it: | --- | git checkout -b development origin/master
Switch from one branch to another: | --- | git checkout <branchname>
List all the branches in your repo, and also tell you what branch you're currently in: | --- | git branch
Delete the feature branch: | --- | git branch -d <branchname>
Push the branch to your remote repository, so others can use it: | --- | git push origin <branchname>
Push all branches to your remote repository: | --- | git push --all origin
Delete a branch on your remote repository: | --- | git push origin :<branchname>	
  |   |  
--- | UPDATE FROM THE REMOTE REPOSITORY | ---
Fetch and merge changes on the remote server to your working directory:	| --- |	git pull
To merge a different branch into your active branch: | --- | git merge <branchname>
View all the merge conflicts: | --- | git diff
View the conflicts against the base file: | --- | git diff --base <filename>
Preview changes, before merging: | --- |git diff <sourcebranch> <targetbranch>
After you have manually resolved any conflicts, you mark the changed file: | --- | git add <filename>
  |   |  
--- | TAGS | ---
You can use tagging to mark a significant changeset, such as a release: | --- | git tag 1.0.0 <commitID>
CommitId is the leading characters of the changeset ID(unique, max 10).Get the ID using: | --- | git log
Push all tags to remote repository: | --- | git push --tags origin
  |   |  
--- | UNDO LOCAL CHANGES | ---
Replace the changes in your working tree with the last content in head: | --- | git checkout -- <filename>
Changes already added to the index, as well as new files, will be kept.	| --- | ---
Fetch the latest history from the server and point your local master branch at it: | --- | git fetch origin
Drop all your local changes and commits: | --- | git reset --hard origin/master
  |   |  
--- | SEARCH | ---
Search the working directory for foo(): | --- | git grep "foo()"
  |   |  
--- | SSH Verify | ---
Remove ssh sertificat: | --- | git config --global http.sslVerify false