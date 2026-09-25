

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

# Viewing staged and unstaged changes
To compare staging area files with previous committed versions

        git diff --staged


To compare working directory with staging files

        git diff


# Visual Diff tools
* Kdiff3
* P4Merge
* Winmerge
* Vscode

To launch the visual diff tool (if its properly configured)

To compare working directory to staging area

        git difftool

To compare staged files with previously commited

        git difftool --staged


NOTE = As we would do with the command git diff

