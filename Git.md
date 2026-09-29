

# Configure GIT

## Levels
* System
* Global
* Local



        git config --global user.name "Luis Fernandes"
        git config --global user.email "something@something.com"
        git config --global core.editor "code --wait"
        git config --global diff.tool vscode
        git config --global difftool.vscode.cmd "code --wait --diff $LOCAL $REMOTE"
        

To open your config file in your default editor

        git config --global -e

To config end of lines properly

        git config --global core.autocrlf true (in windows) / input (in mac)

    

# Initializing a repository

1. Go to the projects directory and then 
2. git init


# Staging Files

1 - To see the status of the current directory and staging area. You'll see that if there are new files they're in red

        git status

2 - To add the files to the staging area

        git add file1.txt file2.txt
        //or
        git add . //to add the entire directory

3 - Run git status again to check that the files are now in green => they're in the staging area
    


# Commiting changes

Now that we have files in the staging are we can commit them by:

        git commit -m "initial commit"

        //or without message to enter the message in the default editor

        git commit

## Skipping staging area

        git commit -am "fix the bug"



# Removing files from the staging area
To check files in the staging area

        git ls-files

If we removed the file in the working directory, we must add it for the staging area to remove that file also. Instead of doing these 2 operations we can:

        git rm file1.txt
    
This will remove it from both the working directory and the staging area.

# Renaming or moving files
The same logic as with the remove. We have the same 2 ops and we can add 1 file to delete it and the other to be created and then commit or:

        git mv main.txt file1.txt (args are filename before / filename after)

    
# Ignoring Files
To ignore file from being tracked

1. Create a file .gitignore in the root of the project
2. Inside that file write what to ignore
3. Add the .gitinore file to the staging area and then commit

        git add .gitignore
        git commit -m "add gitignore"

4. If by mistake the file we didn't want was already commited, we can check if it is on the staging area 

        git ls-files
        git rm --cached -r bin/


# Short Status
As alternative to status we can shorten it by 

        git status -s

It will show something like this:

         M file1.js
        ?? file2.js

* left column represents the staging area
* right column represents the working directory
* M means modified
* ? means new untracked file
* A means added

# Diffing


        git diff --staged       //compare staging area files with previous committed versions

        git diff                //compare working directory with staging files

        git diff HEAD~2 HEAD    //compare Head with 2 commits before

        git diff HEAD~2 HEAD audience.txt    //compare Head with 2 commits before for a specific file

        git diff HEAD~2 HEAD --name-only    //name of the files touched by the commits

        git diff HEAD~2 HEAD --name-status    //name of the files touched by the commits and what happened to each file


# Visual Diff tools
* Kdiff3
* P4Merge
* Winmerge
* Vscode

To launch the visual diff tool (if its properly configured)

To compare working directory to staging area

        git difftool

To compare staged files with previously committed

        git difftool --staged


NOTE = As we would do with the command git diff

# Viewing history
To view the history of commits

        git log

        git log --oneline               //in one line

        git log --oneline -3            //last 3 commits

        git log --oneline --reverse     //in one line but top to bottom

        git log --oneline --author="LF"  //commits by author

        git log --oneline --after="2020-08-17"  //after x date

        git log --oneline --after="yesterday"  //after yesterday

        git log --oneline --after="one week ago"

        git log --oneline --grep="GUI"   //commits with the word GUI in their message

        git log --oneline -S"hello()"    //commits that have changed that line

        git log --oneline -S"hello()" --patch    //to also show the actual changes

        git log --oneline fb0d184..edb3594    //range of commits

        git log --oneline -- file1.cs              //all the commits that touched a file
        
        git log --oneline --patch -- file1.cs      //the actual changes to a file in particular

        git log --oneline --stat        //to get the statistics about the files changed in each commit
        
        git log --oneline --patch       //to show the actual changes

        git log --oneline --all         //to show every commit (even after HEAD if we're in detached HEAD mode)
        
        git log --oneline --all --graph //same but with graph

        git log --stat                  //without oneliners

        git shortlog                    //contributors

        git shortlog -n -s -e           //contributors sorted by commit nr , without commit message and emails

        git shortlog -n --before="" --after=""       //contributors sorted by commit nr and without commit message



HEAD -> master = means that the current branch (HEAD) points to the master branch


# Viewing changes on a commit
To check the changes that went on a commit we can

        git show d601b90 
        
        git show HEAD           //last commit

        git show HEAD~1         //second to last commit

        git show HEAD~1 --name-only         //shows only the names of the files touched by the commit

        git show HEAD~1 --name-status       //shows the names of the files touched by the commit and what happen to each one (modified, deleted)


If instead of the differences you want to see the final version of the file

        git show HEAD~1:bin/app.bin

If you want to see all the files that went on a commit

        git ls-tree HEAD~1


# Git objects
* Commits
* Blobs (Files)
* Trees (Directories)
* Tags

# Restoring Files
To undo the Add operation:

        git restore . 

        git restore --staged file1.js

The restore command takes the copy from the next environment. So:

* Restore on staging environment -> gets the changes from last commit
* Restore on working directory -> gets the changes from the staging environment


Imagining that we deleted a file and then committed the deletion.

        git restore --source=HEAD~1 file1.js //puts the file only in the working directory

        git checkout HEAD~1 file1.js    //puts the file in the working dir and on the index


# Discarding Local Changes
To clean untracked files

        git clean -fd


# Create an Alias
We can create alias for commands in the config

        git config --global alias.unstage "restore --staged ."

        git unstage        //this will perform the configured command


# Checking out a commit
Get the working directory to look like an earlier point in time

        git checkout dad47ed

You'l get a warning saying that you're in detached HEAD state
When you're in this state, it's important not to create a new commit because this will not be reachable and it will get cleaned by git.
To point HEAD back to master...

        git checkout master

# Bisect
Divide and conquer strategy to see where the bug was initially added to the codebase

        git bisect start

and now we have to tag good and bad commits

        git bisect bad          //assuming HEAD already has the bug

and to tag a good commit

        git bisect good ca49180

and we continue until finding the commit that introduced the bug. To end and reatach HEAD to master we do

        git bisect reset



# Blame tool

        git blame -e file1.js          //commit authors with emails


        git blame -e -L 1,3 file1.js   //same but the first 3 lines


# Tagging


        git tag v1.0            //tags HEAD
 
        git tag v1.0 5e7a828    //tags a particular commit

        git tag -a v1.1 -m      //creates an annotated tag

        git tag                 //lists all tags

        git tag -n              //lists all tags and messages associated

        git tag -d v1.0         //deletes the tag

        git checkout v1.0       //checkouts by the tag


# Branches


        git branch bugfix       //creates a new branch

        git branch              //lists all branches

        git switch bugfix       //switches to another branch

        git switch -C bugfix    //creates and switch to a branch

        git branch -m bugfix bugfix/signup-form  //changes the name of the local branch

        git branch -D bugfix    //force delete a branch

        git branch --merged     //lists merged branches that are safe to delete

        git branch --no-merged     //lists unmerged branches

        git log master..bugfix  //what commits are in bugfix but not on master

        git diff master..bugfix //diffing both branches

        //or if we are already on master we can omit it

        git diff bugfix

        git diff --name-status bugfix //now with the filenames and types of changes

# Stashing
When we switch branches git resets our working directory to the snapshot stored in the last commit of the target branch. If we have changes in our working dir that we didn't commit yet but we want to change branch we must first stash them to not lose them.

        git stash push -am "some indicative message"

        git stash list          //lists all stashes

        git stash show 1        //shows the changes in the stash

        git stash apply 1       //applies the stash changes to the working dir

        git stash drop 1        //removes stash

        git stash clear         //removes all stashes


# Merging
There's 2 types of merges

* Fast-forward merges  => if branches have not diverged, just moves master tag to the newest commit of the new branch
* 3-way merges => In case the branches are diverged (master has a new commit that is not in the new branch)looks at the common ancestor and the 2 tips of each branch and combines these last two in a new commit (merge commit).

        git merge bugfix

        git merge --no-ff bugfix        //disables fast forward merge so it creates a merge commit


And because we can forget about the no ff policy we have a way to configure git that way.

        git config --global ff no


# Conflicts
When the automatic merge is not possible and there's a conflict, the merge process halts and we must go in and inspect the conflicts, first we:

        git status

It will show unmerged paths. Now if you open one of those unmerged paths with the default editor, because we're inside the merge process still, it will show us the conflicts, edit those conflicted lines on the code, add the file to the staging dir and then commit.

Aborting a merge:

        git merge --abort

Undoing a faulty merge:
Removing last commit:

        git reset --hard HEAD~1

ATTENTION: THIS REWRITES HISTORY AND SHOULDN'T BE DONE IN CASE OF ANY OF THESE COMMITS ARE ALREADY PUSHED TO REMOTE

* soft - just point the HEAD pointer to the snapshot
* mixed - point and get the snapshot in the staging area
* hard - point and get the snapshot in the staging area and working dir

In case of already pushed to remote, we can revert the last commit

        git revert -m 1 HEAD    //reverts to the first parent (master)


# Merge Tools
* Kdiff
* P4Merge
* Winmerge


        git config --global merge.tool p4merge

        git config --global mergetool.p4merge.path "C..."

        //when in merge process and conflict arises
        git mergetool


# Squash merging
A new commit that combines all the changes on the new branch, deletes the branch and ff the master to it. Use it only with small branches with bad history. Like bugfixes

        git merge --squash bugfix

Attention: as this has no merge commit, we need to delete the branch by hand after.

# Rebase
To get linear history (assuming master is divergent), we can point the base of the new branch to the last commit on master. This operation rewrites history so only use in case it is local or you are certain no one will work on top of that branch.

        git switch feature      //get to the branch to rebase

        git rebase master       //take the base of this branch and point it to the last commit of master

        git merge feature       //now we can ff merge

If theres a conflict while rebasing, we edit the files and then

        git rebase --continue

        git clean -fd   //if we aborted the rebase and theres a temp file

        git config --global mergetool.keepBackup false  //config merge tool to not produce a backup


# Cherry Picking



















