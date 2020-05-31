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

<b>Examples: </b></br>
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
When working on a Git project, sometimes we make changes that we want to get rid of. Git offers a few eraser-like features that allow us to undo mistakes during project creation.
</br></br>
In Git, the commit you are currently on is known as the <b>HEAD</b> commit. In many cases, the most recently made commit is the <b>HEAD</b> commit. 
</br></br>
To see the HEAD commit: 
`git show HEAD`
</br>
The output of this command will display everything the git log command displays for the <b>HEAD</b> commit, plus all the file changes that were committed.

### Git checkout
The command `git checkout filename` reverts committed changes and will restore the file in your working directory to look exactly as it did when you last made a commit. ONLY the <b>HEAD</b> (latest commit)!
</br>
This is your <b>HEAD</b> before checkout: 
```
$ git show HEAD
commit 76a1c2d57b4b4601700b382cef23bdb108d85bb
2
Author: Joeh <ccuser@blabla.com>
Date:   Sun May 31 08:18:21 2020 +0000

    Inserted Ghost to scene5.txt

diff --git a/scene-5.txt b/scene-5.txt
index b12dd97..5dd5d4e 100644
--- a/scene-5.txt
+++ b/scene-5.txt
@@ -12,3 +12,7 @@ Hamlet:
 I will.


+Ghost:
+My hour is almost come,
+When I to sulphurous and tormenting flames
+Must render up myself.
\ No newline at end of file
```

Run `git diff filename` command after doing some changes to scene-5.txt:
```
$ git diff
diff --git a/scene-5.txt b/scene-5.txt
index 5dd5d4e..1aa3f62 100644
--- a/scene-5.txt
+++ b/scene-5.txt
@@ -14,5 +14,5 @@ I will.

 Ghost:
 My hour is almost come,
-When I to sulphurous and tormenting flames
-Must render up myself.
\ No newline at end of file
+When I to sulphurous and tormenting balloons
+Must render up myself.
\ No newline at end of file
```

Reverting changes on HEAD (latest commit!) with `git checkout HEAD filename` and verify with `git diff filename`: </br>
```
$ git checkout HEAD scene-5.txt
$ git diff scene-5.txt
```
</br>

Adding changes on multiple files: 
`git add filename_1 filename_2`

Verify with: `git show HEAD`
</br>
#### git reset I
What if, before you commit, you accidentally delete an important line from scene-2.txt? Unthinkingly, you add scene-2.txt to the staging area. The file change is unrelated to the Larry/Laertes swap and you don’t want to include it in the commit.

We can <i>unstage</i> that file from the staging area using: `git reset HEAD filename1 filename2`</br>
This command <i>resets</i> the file in the staging area to be the same as the <b>HEAD</b> commit. IT DOES NOT DISCARD FILE CHANGES FROM THE WORKING DIRECTORY, IT JUST REMOVES THEM FROM THE STAGING AREA!

You have now added scene-3.txt and scene-7.txt to staging area but not yet committed changes permanently. And now you've made some changes (accidentally deleted some lines of texts and committed scene-2.txt to staging area) to scene-2.txt and after that added scene-2.txt to staging area as well. Check `git status` and scene-2.txt is amongst staged files. Now you want to unstage changes made to scene-2.txt and does so with `git reset HEAD scene-2.txt`</br>
```
$ git reset HEAD scene-2.txt
Unstaged changes after reset:
M       scene-2.txt
```
<b>M</b> is short for Modifications.</br>

Now that your accident in scene-2.txt is removed from staging area you are ready to commit changes permanently `git commit -m "saves scene-3.txt and scene-7.txt permanently"`
</br>
#### git reset II
Creating a project is like hiking in a forest. Sometimes you take a wrong turn and find yourself lost.</br>
</br>
Just like retracing your steps on that hike, Git enables you to rewind to the part before you made the wrong turn. You can do this with:</br>

`git reset commit_SHA`</br>
This command works by using <b>the first 7 characters of the SHA of a previous commit</b>. For example, if the SHA of the previous commit is `5d692065cf51a2f50ea8e7b19b5a7ae512f633ba`, use: </br>

```
git reset 5d69206
HEAD is now set to that previous commit.
```
</br>
<b>Example: </b></br> 

```
$ git reset 76a1c2d
Unstaged changes after reset:
M       scene-2.txt
M       scene-3.txt
M       scene-7.txt
```
Before reset:</br>
HEAD is at the most recent commit</br></br>
After resetting:</br>
HEAD goes to a previously made commit of your choice
The gray commits are no longer part of your project
You have in essence rewound the project’s history. 

![Git reset](https://raw.githubusercontent.com/sthlmj/devnotes/master/Screenshot%202020-05-31%20at%2011.35.38.png)
</br></br>
### Generalizations
Three different ways to backtrack in Git. You can use these skills to undo changes made to your Git project.</br>
</br>
Review the new commands: </br>
`git checkout HEAD filename`: **Discards changes in the working directory**.</br>
`git reset HEAD filename`: Unstages file changes in the staging area.</br>
`git reset commit_SHA`: Resets to a previous commit in your commit history.</br>
Additionally, you now have a way to add multiple files to the staging area with a single command:
</br>
`git add filename_1 filename_2`
</br>

Commonly used shortcut for `git checkout HEAD filename`: </br> 
`git checkout -- filename`
</br>

If you want to commit all staged files: git add .
```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will
 be committed)
  (use "git checkout -- <file>..." to discard
changes in working directory)

        modified:   house.txt
        modified:   portrait.txt
        modified:   tree.txt

no changes added to commit (use "git add" and/
or "git commit -a")
$ git add .
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   house.txt
        modified:   portrait.txt
        modified:   tree.txt
```

**Recap:** </br>
Git backtracking commands lets you: discard changes in the working directory, go back to a previous commit, unstage a file in staging area. </br>
`git checkout HEAD filename` restores a file in the working directory to look as it did in your last commit. </br>
`git reset HEAD filename` command removes file changes from the staging area.</br>


## Git - Git Branching 
Up to this point, you’ve worked in a single Git branch called master. Git allows us to create branches to experiment with versions of a project. Imagine you want to create version of a story with a happy ending. You can create a new branch and make the happy ending changes to that branch only. It will have no effect on the master branch until you’re ready to merge the happy ending to the master branch.
</br></br>
Using Git branching to develop multiple versions of a resumé.
</br>
You can use the command below to answer the question: “which branch am I on?”
`git branch`
</br>
The diagram illustrates branching.

The circles are commits, and together form the Git project’s commit history.
New Branch is a different version of the Git project. It contains commits from Master but also has commits that Master does not have.

![Git branching](https://github.com/sthlmj/devnotes/blob/master/Screenshot%202020-05-31%20at%2012.44.34.png?raw=true)
</br>

### git merge
Merging newly created fencing branch onto master. </br>
`git merge branch_name` example merging fencing onto master: `git merge fencing`
</br>
Your goal is to update **master** with changes you made to **fencing**.</br>
**fencing** is the giver branch, since it provides the changes.</br>
**master** is the receiver branch, since it accepts those changes.</br>


## Git - Git Teamwork

## TODO - Moving git repository with full history: </br>
https://www.atlassian.com/git/tutorials/git-move-repository </br>
https://gist.github.com/niksumeiko/8972566 </br>
https://medium.com/@zeebaig/how-to-migrate-git-repository-with-branches-and-commit-history-dd129fd36dca </br>
https://medium.com/@ayushya/move-directory-from-one-repository-to-another-preserving-git-history-d210fa049d4b </br>


## TODO - Configure nginx redirect with docker-compose to handle if someone forgot to configure new remote repo url: </br>
