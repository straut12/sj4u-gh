---
layout: page
permalink: /ref/coding/git/
title: VScode and Github
description: Intro to VScode and Github
nav: false
toc: true
---
For Linux I install with ```sudo apt install git```  
For Windows I install from the git download page. You can then run a git cmd terminal and have similar functions as a linux terminal. You can also use the vs code to run git commands instead of a terminal.

# VS code with git
There is a large number of code editors to choose from. [VScode](https://code.visualstudio.com/download) is a popular choice because of its  extensions, light enough to run on RPi3-4, and integrates well with git.​  

With VS code extensions you can essentially turn it into an IDE. At least for STEM project purposes. For Raspberry Pi the primary extensions I install are Python, Remote-SSH, and GitHub Pull Requests and Issues.
* With remote-ssh you can connect from your main PC to any RPi (except a Pi0) and code a project. This eliminates having to hook a monitor up to the RPi and is smoother than vnc'ing to the RPi (especially a RPi3/A).
* Use git to stay sync'd with your github account.  

----------------------------

**User and Workspace Settings**  
To avoid "unresolved import 'packageA' when importing your own modules in VS code  
Workspace settings -   
{ "python.autoComplete.extraPaths": ["./path/to/packageA"] }  

--------------------------------

**git** is the version control portion and tracks the history of your code/project. Often you create a simpler, first version of your project that works, but doesn't have all the features you ultimately want. You want to add more features but without breaking your working or production code (main branch). Git allows you to create additional branches where you can add features or fix bugs and then merge it back into your main branch.

**github** is the cloud portion where you can remotely store your project in Git repositories. Thru extensions, VScode has built-in git functions which help make the process even easier (currently microsoft owns both). Using VS code with git hub extensions you can easily keep your project code sync'd with the cloud so you always have an up-to-date backup and can easily clone it to another system.

A couple good articles on the functions of git and version control are at [reflectoring](https://reflectoring.io/github-fork-and-pull/) and [adafruit](https://learn.adafruit.com/contribute-to-circuitpython-with-git-and-github?view=all).

Other code editors I looked at  
* Pycharm **~800MB** (too much for a Pi3/1GB RAM)
* VScode  **~300MB**
* Thonny, mu-editor, geany, nano. **< 50MB**  
* Memory/RAM comparison using ```$ free -m```  

----------------------------

# Setup Including SSH Key
**Installing VScode** - VScode is now supported by Raspberry Pi and can be installed by downloading the .deb at [VScode Downloads](https://code.visualstudio.com/download)  
Download .deb ARM (or ARM 64 if using 64 bit version)  
Open the file with gdebi to install or run command  
```$ sudo gdebi <vscode.deb file>```  
(VS code is also in Ubuntu software center to install on Ubuntu)  

**Installing git**  
```$ sudo apt-get install git```  
Needed gnome-keyring below for the VScode git functions  
```$ sudo apt install gnome-keyring ```  
Go to extensions in VScode and install the **GitHub Pull Requests and Issues**  (you will also need the Python extension if working Python projects)  
One time setup to set some git defaults for your repositories  
```$ git config --global user.email "you@example.com"```  
```$ git config --global user.name "Your Name"```  

**ssh Key Setup**  
To automatically sync your local code with github you'll need to securely link your RPi to your github account. This can be done using ssh keys. Follow the detailed steps at [SSH key authentication](../../../ref/linux/vnc-ssh/#ssh-key-authentication)  

You will need to do these steps for each RPi or other local system you want to use to update code on your remote GitHub account.  

**Some terms in github**  
HTTP = read only (you can also download a zip file and extract it)  
SSH = read/write (the write portion requires a SSH Key)  
​​​​​​Synchronizing involves **pull**ing remote changes down to your local repository and then **push**ing any local commits back to the upstream branch.

----------------------------

# VS code git Workflow  
For starting an initial project I create an empty repo in github and then clone it to my local system where it will be running. Then I use VS code to work on the code and keep it sync'd with github.  

Steps are assuming a RPi will be running the project code  
1. Login to [github](https://github.com/) and create a new, empty, repository. I also go ahead and create a README and use the .gitignore python template.  
2. Click on the **Code** button and copy the **SSH link**. (GitHub account must be linked with [SSH key](../../../ref/linux/vnc-ssh/#ssh-key-authentication) to the RPi)  
3. Here you can either go directly to the RPi or use VS code with remote-ssh on a PC/laptop and connect remotely to the RPi.   
Once connected clone the empty repository using the copied SSH link. If in the VScode GUI hit F1 to get the command pallette and type "git". Or at a VS code or system terminal ```CD <dir>``` into your working directory and  
```$ git clone git@github.com:user/repo.git```  
4. In VS code navigate to the project folder  
5. You can start a **test.py** to edit and test the git functionality  
    * Go to 'Source Control' option (left side bar) to see the files. As you create/edit files they will show up here for you to stage/commit for later sync'ing with github. New files will be listed with a 'U' for untracked, modified files with a M, deleted files with a D, etc.   
    * If working with a virtual env your .gitignore should show it will ignore the venv files when uploading to github. (you normally don't want these files uploaded). If you didn't use the python .gitignore template in step 1 you'll need to right click on the .venv folder and manually add it to the .gitignore. To see the .venv as a folder you may have to click on the "Toggle View Mode" at the top.  
    * You can click on a modified file to see what changes you've made (working tree). Or click the undo/discard changes icon to undo updates from a file.
6. When finished updating your code you go back to 'Source Control', add a comment, and then 'commit' and 'sync'. (you can hit shift-enter to add multiple lines of comments). There is an icon in the bottom left for synchronizing changes too. Or terminal commands are  
    * ```$ git diff```  
    * ```$ git add . ```  
    * ```$ git diff --staged```  
    * ```$ git commit -m "message comments"```  
    * ```$ git push -u origin main ```(-u will set the upstream default so in future you can just type $ git push)  
7. Now you can go to the [github](https://github.com/) repo, refresh, and confirm the changes were uploaded

---------------------------------

If you didn't clone from github and have an existing project you want to link to a remote GitHub repository you have multiple options.  
* From command palette type 'git add remote' and select it. Now you can paste the **SSH link** from github
* or a friendlier method is select the 'Add remote from GitHub' provided by the GitHub Pull Request and Issues extension. This will log you into GitHub and then show you your repositories so you can select one.  For the name use 'origin'.  
* Or in a terminal type  
```$ git remote add origin <copied SSH link>```  
Now the local Pi folder is linked to remote GitHub repository. You can always check with command  
```$ git remote -v```  

------------------------------------------------

To download the code to another RPi do the following in a terminal or use the key words to navigate the VS code git menus  
1. ```$ cd ```(go to directory one branch up from where you want to install.. clone command below will create the folder)
2. ```$ git clone <SSH link from remote GitHub>​```  
3. ```$ git remote -v ```(confirm origin link)  

------------------------------------------------

# git Branches  
Branches are useful for adding features or fixing bugs. It allows you to leave your working, production code alone by branching off. When you're done with the branch you can merge the updates back to the main branch.  

Git branch commands for local  
```$ git branch ``` (list branches)  
```$ git checkout -b <new branch> ``` (creates new branch and checks it out, $ git checkout)  
```$ git branch -d <branch to delete> ``` (use for -D to force delete)  
```$ git branch -m <new name>```   
```$ git branch -a ``` (list remote branches)  

**Create a permanent "develop' branch**  
Develop acts as a work-in-progress version of you main branch. You can build features or fix bugs from the develop. (you will always merge back to the main branch when features/bug fixes are completed)  
```$ git checkout -b develop main```   

**Create a "feature" branch from develop**  
```$ git checkout -b feature1 develop ``` (creates feature branch and checks it out)  
Can work on the feature and make commits  

**Merge the feature back into develop branch**  
```$ git checkout develop ``` (switch from feature back to develop)  
```$ git merge --no-ff feature1 ``` (merge develop and feature1)  
```$ git branch -d feature1 ``` (delete the feature1 branch)  
```$ git push origin develop ``` (update the remote repo)  

**When all features are complete merge develop back into main**  
```$ git checkout main```   
```$ git merge develop```   

-----------------------------------------------

**Create a "hotfix" (bug) branch from main**  
```$ git checkout -b hotfix1 main ``` (creates hotfix branch and checks it out)  
Can work on the bug and make commits  

**Merge the bug fix back into main and develop branch**  
```$ git checkout main```   
```$ git merge --no-ff hotfix1```   
```$ git checkout develop```   
```$ git merge --no-ff hotfix1```   

```$ git push origin develop ``` (update the remote repo)  
```$ git push origin main ``` (update the remote repo)  

```$ git branch -d hotfix1 ``` (delete the hotfix branch)  

------------------------------------------

# Other Useful git Commands  
Many of the git commands are accessible in VScode in the command palette (ctrl-shift-P) and start typing git ..
.git/config (in your home directory) is also useful to look at  

To see a log of changes  
```$ git log --oneline --decorate```  

## Rename a Repo
**Rename a Repo**  
At github go to settings and rename the repo 
On local PC  
```$ git remote set-url origin git@github.com:user/newname.git```  
​
Note you may have to do the following  
```$ git remote rm origin```  
```$ git remote add origin git@github.com:user/newname.git```  

General commands  
```$ git init``` (set a folder up for git)  
```$ git status ``` (Check status of files committing)  
```$ git add .```  
```$ git commit -m "message details"```  


------------------------------------------------


**​HEAD** is the current branch (active head). When you switch branches with git checkout, the HEAD revision changes to point to the tip of the new branch.  
To see what HEAD points to  
```$ cat .git/HEAD```  


------------------------------------------------

## Delete a git folder
**Close/Remove a git folder from a local project**  
In VScode palette (ctrl-shift-P) type Git: Close Repository  
Then delete the .git directory  
```$ rm -rf .git/```  

Cleaning up a GitHub repository - Delete files on remote GitHub repository (but leaves them on local system)  
```$ git rm -r .venv2/ --cached  ``` --cached leaves file on local system. can also add --dry-run flag at end to see what will be removed before doing it  
If you omit the --cached it will remove both the local and remote files.  


------------------------------------------------


**Git Remote Commands (syncing - push/pull)**  
Remote commands  
Can see the github link under the github repo and Code (clone) button  
```$ git clone <git http> ``` (will create remote link)  
HTTP is read only and SSH is read-write  

```$ git remote -v ``` (verbose)  
```$ git remote add origin <SSH link>```  
```$ git remote rm origin```  
```$ git remote rename origin new-origin ```(can also edit .git/config)  
```$ git remote show origin```  

If there is no remotes/origin/HEAD -> origin/main run   
```$ git remote set-head origin -a```   
remotes\origin\HEAD indicates the default branch on the remote. You can use origin as a shorthand for origin/master.   

other remote/origin commands  
```$ git remote remove origin  ``` To remove link to remote  
```$ git remote set-url origin <new SSH link> ``` To change the remote link (can also edit .git/config and change URLs)  
Upload local repo content to a remote repo  
```$ git push <remote> <branch>```  
```$ git push -u origin main ```(future can type $ git push)  
origin' is just an alias on your local system for a particular remote repository. You can also push using the URL directly  
```$ git push <SSH link> main```  
```$ git push origin -all ``` (push all local branches)  
```$ git push origin --force ``` (forces the push even if it results in non-fast-forward merge)  

## Fix Remote Local Diverged
**If remote history has diverged from local history then pull the remote and merge it into the local, then push again.**  

```$ git fetch origin ``` (download commits, files, refs from the remote repo but doesn't automatically merge them)  
    * ```$ git fetch origin <branch-name>  ``` (for specific branch)  
```$ git log --oneline main..origin/main ``` (to see changes from remote)  
```$ git checkout main ``` (updates the files in working dir to match the version stored in main and all new commits will go to main)  
```$ git log origin/main```   
```$ git merge origin/main ``` (main and origin/main now point to the same commit)  
```$ git pull origin ``` (fetch + merge: Will fetch changes based on where local branch HEAD is pointed. Then merges into local repo and updates HEAD to point to the new commit)  


------------------------------------------------


A **gitignore** file allows you to specify what files/folders you want to prevent from uploading to the github remote repo. To configure a global .gitignore (this will add a line in your .gitconfig)  
```$ touch ~/.gitignore_global```   
```$ git config --global core.excludesfile ~/.gitignore_global```   
```console
.vscode/
workspace.code-workspace
.venv/
*.log
.log
```
​To update a file already in your repo that you want to ignore.  
```$ git update-index --assume-unchanged <file>```   


------------------------------------------------


**Rename the Default git Branch**  
To change from master to main branch.  
Go to github and repository settings for the project.  
Then branches and edit. Rename the branch.  
Then if you have a local clone run  
```$ git branch -m master main```   
```$ git fetch origin```   
```$ git branch -u origin/main main​```   
```$ git remote set-head origin -a```   

options below may be used for trouble shooting.  

VScode bottom left 'master' button. + Create new branch from .. (select master to copy from and name it 'main')  
then select HEAD ref  

From terminal  
Starting with master default branch  
```$ cat .git/HEAD --> ref: refs/heads/master ```   
```$ git branch -a ```   
```console
* master 
remotes/origin/HEAD -> origin/master 
remotes/origin/master 
```

```$ git branch -m master main -m ``` to move the master branch to main, including the history  
```$ cat .git/HEAD --> ref: refs/heads/main ```   
and  ```$ git branch -a ```   
```console
* main 
remotes/origin/main 
remotes/origin/master 
```

```$ git push -u origin main  ``` Push the new main branch to remote GitHub repo  
```console
remote: Create a pull request for 'main' on GitHub by visiting: 
remote: https://github.com/xxxxx/pull/new/main 
remote: 
To github.com:xxxxxx.git 
* [new branch] main -> main 
Branch 'main' set up to track remote branch 'main' from 'origin'.  
```

```$ git symbolic-ref refs/remotes/origin/HEAD refs/remotes/origin/main  ```   
Point or switch current HEAD to main branch then  
```$ git branch -a```    
```console
* main 
remotes/origin/HEAD -> origin/main 
remotes/origin/main 
remotes/origin/master
```

Now go to GitHub and set the default to main under settings/branches. Then go back to terminal to delete the master  

```$ git push origin --delete master ```  Delete the master   
```$ git branch -a ``` to confirm the master branch was removed  
```console
* main 
remotes/origin/HEAD -> origin/main 
remotes/origin/main 
```

--------------------------

to manually update the tracking branch (listed a starting point at the end)  
```$ git branch -u origin/main main```   

-------------------------

If there are other Pi's with local clones will need to update them  
```$ git checkout master ```  switch to master branch  
```$ git branch -m master main ```  move history to main locally  
```$ git fetch  ``` get latest commits from server  
```$ git branch --unset-upstream ```  remove the link to origin/master   
```$ git branch -u origin/main  ``` add a link to origin/main  
```$ git symbolic-ref refs/remotes/origin/HEAD refs/remotes/origin/main```  update default branch to origin/main  


-----------------------------  
-----------------------------  
