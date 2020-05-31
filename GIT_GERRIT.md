# Git cheatsheet!
Collection of git command cheatsheet - in details!

## Git - Basic git workflows

`git init`
The word init means initialize. The command sets up all the tools Git needs to begin tracking changes made to the project.

A Git project can be thought of as having three parts:

A Working Directory: where you’ll be doing all the work: creating, editing, deleting and organizing files
A Staging Area: where you’ll list changes you make to the working directory
A Repository: where Git permanently stores those changes as different versions of the project
The Git workflow consists of editing files in the working directory, adding files to the staging area, and saving changes to a Git repository. In Git, we save changes with a commit, which we will learn more about in this lesson.

`git status`
In the output, notice the file in red under untracked files. Untracked means that Git sees the file but has not started tracking changes yet.

`git add`
In order for Git to start tracking scene-1.txt, the file needs to be added to the staging area.

`git diff filename`
Here, filename is the actual name of the file. If the name of my file was changes.txt the command would be

`git add filename` 
Adds the changes in filename to the staging area in Git.

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

## Git - How to Backtrack in Git


## Git - Git Branching


## Git - Git Teamwork
