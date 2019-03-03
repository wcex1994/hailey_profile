## Source Control

Source control is a key part of data engineering. As the title "data engineering" itself explains, it has many similarity to software engineering, but with a different focus. This is a summary of the reading in the reference. It is good for people who just enter Git world.

### Benefit

The benefits for source control include:

* **Change tracking**: You can see what is exactly changed and metadata will help a lot for future troubleshooting
* **Accountability**: It is clear who makes the change and it will hold everyone for responsibility.
* **Process and workflow**: It ensures the process is healthy. For example, pull-request can enable review beofre a post.

### Terminology

THere are few Git terminology:

* **Repository**: Database contains all of a project’s information (files and metadata) and history.
* **Working directory**: This is the directory where you, as the user of Git, will modify the files contained in the repository.
* **Index**: Repo’s directory structure at a point in time.
* **Commit**: An entry in the repo recording metadata for each change introduced to the repo.

### Architecture

By default, this information is stored in a directory named **.git** in the root of your working directory (this behavior can be changed). You should **NEVER** interact with this directory directly.

* Actual repository in the .git subdirectory
* Index in .git/index
* Content-addressable objects in .git/objects
* Repo specific configuration details in .git/config
* Metadata, change, and objects in .git/logs

### Common Git Commands

Help (two options)

    ```
    git help <command>
    man git-commit #put a dash in the git command
    ```

Clone

Repository is the location of the repository you’re cloning
Directory is the (optional) directory where you’d like to place the cloned repository

    ```
    git clone repository directory
    ```

Creating repository

    ```
    mkdir NAME_OF_DIRECTORY #make the folder
    cd NAME_OF_DIRECTORY #go to the folder
    git init #create the .git direcotry and all related subdirectory
    ls -la #show all in the folder
    ```

Remove repository

    ```
    rm -rf repository
    ```

Adding files to a repo

    ```
    # 1. Add files to the working directory
    cp SOURCE DEST #on Mac and Linux
    copy *.txt c:\ #on Windows

    # 2. Show status
    git status # tell you untracked files in working directory and not in the repo

    # 3. Stage the files to the repo's index
    git add FILE # now GIt's index is in sync with the working directory

    # 4. commit the staged files to repo
    # 4.1 provide user info to git (for once)
    git config --global user.name "John Smith" #if repo-specific value, ignore --global
    git config --global user.email "john.smith@networktocode.com"  #if repo-specific value, ignore --global

    git commit -m "First commit to new repository" #-m means the message for your commit, can be anything, very helpful for future debugging 

    git log #you can see your commit in the log

    git commit --amend #change a commit
    git commit -a #-a add all changes from all known files


    # 5. Push
    git push origin master
    ```

Unstaging files

    ```
    git reset HEAD sw5.txt #HEAD is the pointer referencing the last commit you made 
    ```

Excluding files per repo

    ```
    # 1. create empty .gitignore file and edit it and add credential.yml on a single line in the file 
    touch .gitignore
    # 2. commit it as usual

    # Additional: To exclude globally, create .gitignore_global file in home directory and add exclusions to that file
    git config --global core.excludesfile /path/to/.gitignore_global
    ```

Tree

    ```
    git ls-tree SHA_Hash
    ```

Log

    ```
    git log --oneline
    git log start SHA..end SHA #look at specific commit
    git log HEAD~1..HEAD #use HEAD to reference HEAD~1 is the commit just before HEAD

    git cat-file -p 2a656c3 #drill into one specific commit info -p is a format
    ```

Difference

    ```
    git diff 9547063..679c41c sw1.txt #two commits and the file to compare
    it diff --cached #if staged but not committed yet
    ```

Branching

    ```
    git branch NAME_OF_BRANCH
    git checkout NAME_OF_BRANCH

    # or create and check out at the same time
    git checkout -b NAME_OF_BRANCH

    # merge to master
    git merge NAME_OF_BRANCH

    # delete a branch
    git branch -d branch-name
    ```

Remotes
A remote is a reference to another repo.

    ```
    git remote add NAME ~/net-auto #NAME is purly symbolic, location of the remote repo in this case is on the same system

    # Now you have asymmetric link between the two remotes

    # fetch
    git remote update NAME
    git branch -a #now you will have a remote tracking branch which you cannot change or commit to it. It is only a remote reference. 

    git remote add -f NAME location #do the previous in one step

    git fetch NAME #now updated but not in my master yet
    git checkout master
    git merge NAME/master #now two masters are in sync

    git pull NAME #combine git fetch and git merge 
    ```

### Reference

Reading Source: <https://learning.oreilly.com/library/view/network-programmability-and/9781491931240/ch08.html#sourcecontrol>
Linux cp command: <https://www.cyberciti.biz/faq/copy-command/>
Windows Copy Command: <https://www.computerhope.com/copyhlp.htm>