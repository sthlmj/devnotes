# Git cheatsheet!
Collection of git command cheatsheet - in details!

## Git - Basic git workflows

`git init`
The word init means initialize. The command sets up all the tools Git needs to begin tracking changes made to the project.

A Git project can be thought of as having three parts:

A Working Directory: where you’ll be doing all the work: creating, editing, deleting and organizing files </br>
A Staging Area: where you’ll list changes you make to the working directory</br>
A Repository: where Git permanently stores those changes as different versions of the project</br>
The Git workflow consists of editing files in the working directory, adding files to the staging area, and saving changes to a Git repository. In Git, we save changes with a commit.

`git status`
In the output, notice the file in red under untracked files. Untracked means that Git sees the file but has not started tracking changes yet. Also it shows untracked files and file changes staged for commit.

`git add`
In order for Git to start tracking scene-1.txt, the file needs to be added to the staging area. This adds all files to the staging area!  

`git diff filename`
Here, filename is the actual name of the file. If the name of my file was changes.txt the command would be

`git add filename` 
Adds the changes in filename to the staging area in Git. </br> Important to note that safest is NOT TO SKIP `git add filename` on already tracked files that were updated! As the git commit actually stores changes permanently from staging area inside the repository!

`git commit -m "Complete first line of dialogue"`
A commit is the last step in our Git workflow. A commit permanently stores changes from the staging area inside the repository.

`git log`
Often with Git, you’ll need to refer back to an earlier version of a project. Commits are stored chronologically in the repository and can be viewed with `git log`
In the output, notice:
A 40-character code, called a SHA, that uniquely identifies the commit. This appears in orange text.
The commit author (you!)
The date and time of the commit
The commit message

### Generalizations
Git is the industry-standard version control system for web developers
Use Git commands to help keep track of changes made to a project:
* `git init` creates a new Git repository
* `git status` inspects the contents of the working directory and staging area
* `git add` adds files from the working directory to the staging area
* `git diff` shows the difference between the working directory and the staging area
* `git commit` permanently stores file changes from the staging area in the repository
* `git log` shows a list of all previous commits, newest on top. Check time stamps.
* Purpose of Gits staging area is to stage file changes for a commit. A staging area is: a safe place that will allow you to test any changes - major or minor - that you plan on implementing in a secure environment, preventing any unexpected errors on your live/production/secure area - where you dont want to mess up. So that you can go to sleep at nights without headache! 

### Examples: </br>
1. Adds untracked files to staging area: </br> `git add *.txt` </br>
2. re-add files to staging area when changes are made to already tracked files: </br>
`git add disclaimer.txt` </br>
3. Committing staged files permanently: `git commit -m "Added warning txt"`
4. `git log` again to see changes, newest on top:

```
$ git log
commit 3601380b9d6508dba745b90e413357a877d8551
e
Author: JoeH <ccuser@blabla.com>
Date:   Sun May 31 07:13:41 2020 +0000

    Added warning text

commit 7b28b205f22d26e963dde81dddc55f84fc51e14
9
Author: JoeH <ccuser@blabla.com>
Date:   Sun May 31 07:07:38 2020 +0000

    committing files
```

### Git and Github
Git is a widely-used version control system used to manage code. Git allows you to save drafts of your code so that you can look back at previous versions and potentially undo complicated errors. A project managed with Git is called a Git repository.

GitHub is popular hosting service for Git repositories. GitHub allows you to store your local Git repositories in the cloud. With GitHub, you can backup your personal files, share your code, and collaborate with others.

In short, GitHub is a tool for working with Git. There are other services to host Git repositories, but GitHub is a trusted, free service used by organizations across the world, big and small.
</br></br>
Connecting local repository to github: </br>
1. Initialize, add and commit repository. Check git status, there should be no files in staging area - it all should be committed. </br>
2. Create new github repository. </br>
3. Copy and paste "...push an already existing repository HTTPS/SSH" `git remote add origin https://github.com/sthlmj/devnotes` to connect local repository to github "remote repository" </br>
4. `git push -u origin master` pushes local repository into the github remote repository. </br>


## Git - How to Backtrack in Git


## Git - Git Branching


## Git - Git Teamwork

## TODO - Moving git repository with full history: </br>
https://www.atlassian.com/git/tutorials/git-move-repository </br>
https://gist.github.com/niksumeiko/8972566 </br>
https://medium.com/@zeebaig/how-to-migrate-git-repository-with-branches-and-commit-history-dd129fd36dca </br>
https://medium.com/@ayushya/move-directory-from-one-repository-to-another-preserving-git-history-d210fa049d4b </br>


## TODO - Configure nginx redirect with docker-compose to handle if someone forgot to configure new remote repo url: </br>
