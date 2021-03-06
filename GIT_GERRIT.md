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
</br></br>
Keep in mind: Update **master** with changes you made to **fencing**.</br>
**fencing** is the giver branch, since it provides the changes.</br>
**master** is the receiver branch, since it accepts those changes.</br></br>

Switching branch: `git checkout master`</br>

```
$ git checkout master
Switched to branch 'master'
$ git branch
  fencing
* master
$ git merge fencing
Updating 79a1cc5..eef97f2
Fast-forward
 resume.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
$ git branch
  fencing
* master
```
### git merge conflict 
The merge was successful because master had not changed since we made a commit on fencing. Git knew to simply update master with changes on fencing. </br></br>

What would happen if you made a commit on master before you merged the two branches? Furthermore, what if the commit you made on master altered the same exact text you worked on in fencing? When you switch back to master and ask Git to merge the two branches, Git doesn’t know which changes you want to keep. This is called a merge conflict.</br>

Git uses <<<<<<< markings >>>>>>> to show Master and branch differences: </br>
```
<<<<<<< HEAD
master version of line
=======
fencing version of line
>>>>>>> fencing
```

In our case: 
```
<<<<<<< HEAD
-Engage in swordfights with professional pirates
=======
-Engage in swordfights with professional pirates such as Smee.
>>>>>>> fencing
```

Solving merge conflict in the editor: </br>
Delete the content of the line as it appears in the master branch</br>
</br>
Delete all of Git’s special markings including the words **HEAD** and **fencing**. If any of Git’s markings remain, for example, **>>>>>>>** and **=======**, the conflict remains!</br>
</br>
After resolving merge conflict, add the file to staging area with `git add resume.txt` and then commit to merge with: 
```
$ git commit -m "Resolving merge conflict"
[master 3517438] Resolving merge conflict
```

### git delete branch
In Git, branches are usually a means to an end. You create them to work on a new project feature, but the end goal is to merge that feature into the master branch. After the branch has been integrated into master, it has served its purpose and can be deleted.
</br></br>
The command
</br>
`git branch -d branch_name`
will delete the specified branch from your Git project.
```
$ git branch -d fencing
Deleted branch fencing (was 9c955d3).
``` 

### Generalizations 
Let’s take a moment to review the main concepts and commands before moving on.
</br>
Git branching allows users to experiment with different versions of a project by checking out separate branches to work on.
</br></br>
The following commands are useful in the Git branch workflow.
</br>
`git branch`: Lists all a Git project’s branches.</br>
`git branch branch_name`: Creates a new branch.</br>
`git checkout branch_name`: Used to switch from one branch to another.</br>
`git merge branch_name`: Used to join file changes from one branch to another.</br>
`git branch -d branch_name`: Deletes the branch specified.</br>
`git branch -D branch_name`: Delete unmerged branch.</br>
</br>

Fast forward merge, A fast-forward merge can occur when there is a linear path from the current branch tip to the target branch. Instead of “actually” merging the branches, all Git has to do to integrate the histories is move (i.e., “fast forward”) the current branch tip up to the target branch tip: 
```
$ git branch
  master
* unordered-list
$ git checkout master
Switched to branch 'master'
$ git merge unordered-list
Updating 1481f5a..d1a4627
Fast-forward
 index.html | 8 +++++++-
 1 file changed, 7 insertions(+), 1 deletion(-
)
```

## Git - Git Teamwork 
So far, we’ve worked on Git as a single user. Git offers a suite of collaboration tools to make working with others on a project easier.

Imagine that you’re a science teacher, developing some quizzes with Sally, another teacher in the school. You are using Git to manage the project.

In order to collaborate, you and Sally need:

A complete replica of the project on your own computers
A way to keep track of and review each other’s work
Access to a definitive project version
You can accomplish all of this by using remotes. A remote is a shared Git repository that allows multiple collaborators to work on the same Git project from different locations. Collaborators work on the project independently, and merge changes together when they are ready to do so.

### git clone
In order to get a replica of remote project code on your own computer we use `git clone remote_location clone_name`. Where `remote` location tells Git where to go to find the remote. This could be a web address, or a filepath and `clone_name` is the name you give to the directory in which Git will clone the repository.

**Examples:** </br>
```
$ git clone science-quizzes my-quizzes
Cloning into 'my-quizzes'...
done.
```
Git informs us that it’s copying everything from science-quizzes into the my-quizzes directory.
my-quizzes is your local copy of the science-quizzes Git project. If you commit changes to the project here, Sally will not know about them.

### git remote -v
One thing that Git does behind the scenes when you clone science-quizzes (into your my-quizzes) is give the **remote address** the name **origin**, so that you can refer to it more conveniently. In this case, Sally’s remote is origin.

See a list of a Git project’s remotes with the command: `git remote -v` </br>

**Examples:** </br>
```
$ git remote -v
origin  /home/ccuser/workspace/curriculum/science-quizzes (fetch)
origin  /home/ccuser/workspace/curriculum/science-quizzes (push)
```
* Git lists the name of the remote, `origin`, as well as its location.
* Git automatically names this remote `origin`, because it **refers to the remote repository of origin**. However, it is possible to safely change its name.
* The remote is listed twice: once for `(fetch)` and once for `(push)`. We’ll work with these later.

### git fetch
After you cloned science-quizzes, you had to run off to teach a class. Now that you’re back at your computer, there’s a problem: what if, while you were teaching, Sally changed the science-quizzes Git project in some way. If so, your clone will no longer be up-to-date. </br></br>

An easy way to see if changes have been made to the remote and bring the changes down to your local copy is with:</br>
`git fetch` </br></br>

This command <i>will not merge changes from the remote into your local repository</i>. It brings those changes onto what’s called a <i>remote branch</i>. 
```
$ git fetch
remote: Counting objects: 5, done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 5 (delta 1), reused 0 (delta 0)
Unpacking objects: 100% (5/5), done.
From /home/ccuser/workspace/curriculum-a/science-quizzes
 * [new branch]      master     -> origin/master
$ git branch -a
* master
  remotes/origin/master
```

### git merge
Even though Sally’s new commits have been fetched to your local copy of the Git project, those commits are on the `origin/master` branch. Your <i>local</i> `master` branch has not been updated yet, so you can’t view or make changes to any of the work she has added.

In Part III, Git Branching we worked with how to merge branches. Now we’ll use the git merge command to **integrate origin/master into your local master branch**. 
</br></br>
Accomplish this with: 
`git merge origin/master`

Example HEAD is "Add first question to physics quiz", mergin remote/origin/master into local master (local master is receiver of changes): 
```
$ pwd
/home/ccuser/workspace/curriculum-a/my-quizzes

$ git show --
commit 2fd7d9b248e0b4a3b531b9af3bb61916d42ad45f
Author: danasselin <johndoe@example.com>
Date:   Thu Oct 29 15:42:55 2015 -0400

    Add first question to physics quiz

diff --git a/physics.txt b/physics.txt
index e69de29..3d47f43 100644
--- a/physics.txt
+++ b/physics.txt
@@ -0,0 +1,3 @@
+1. A scalar is a quantity which has both magnitude and direction.
+a) true
+b) false
\ No newline at end of file

$ git branch -a
* master
  remotes/origin/master

$ git merge origin/master
Updating 2fd7d9b..3a29454
Fast-forward
 biology.txt | 4 ++++
 1 file changed, 4 insertions(+)
 create mode 100644 biology.txt

```

Git log before merging remote master with local master: 
```
$ git log
commit 2fd7d9b248e0b4a3b531b9af3bb61916d42ad45f
Author: danasselin <johndoe@example.com>
Date:   Thu Oct 29 15:42:55 2015 -0400

    Add first question to physics quiz

commit 2c4e484e0f5bda111a164e6580f4a4a603cea48f
Author: danasselin <johndoe@example.com>
Date:   Thu Oct 29 15:42:36 2015 -0400

    Add physics quiz

commit 65be0d3f24fb34363bdac6949325f35ad3cdd98a
Author: danasselin <johndoe@example.com>
Date:   Thu Oct 29 15:42:21 2015 -0400

    Add chemistry quiz

commit 9bfc082a0e64efe5c0a2e4298e0b4f127d0a9b96
Author: danasselin <johndoe@example.com>
Date:   Thu Oct 29 15:42:00 2015 -0400
```

Git log after merging remote master with local master: 
```
$ git log
commit 3a294546f4a55f02bf37233ef8988d8b9dd7ce59
Author: danasselin <johndoe@example.com>
Date:   Tue Nov 3 12:33:23 2015 -0500

    Add heading and comment to biology quiz

commit 6aa7704a31d05541141fbb529abf946bd2fd416b
Author: danasselin <johndoe@example.com>
Date:   Thu Oct 29 17:04:04 2015 -0400

    Add biology quiz

commit 2fd7d9b248e0b4a3b531b9af3bb61916d42ad45f
Author: danasselin <johndoe@example.com>
Date:   Thu Oct 29 15:42:55 2015 -0400

    Add first question to physics quiz

commit 2c4e484e0f5bda111a164e6580f4a4a603cea48f
Author: danasselin <johndoe@example.com>
Date:   Thu Oct 29 15:42:36 2015 -0400

    Add physics quiz
```
### Git workflow
Now that you’ve merged `origin/master` into your `local master` branch, you’re ready to contribute some work of your own. The workflow for Git collaborations typically follows this order:

1. Fetch and merge changes from the remote
2. Create a branch to work on a new project feature
3. Develop the feature on your branch and commit your work
4. <u>Fetch and merge from the remote again (in case new commits were made while you were working)</u>
5. Push your branch up to the remote for review

**Steps 1 and 4 are a safeguard against merge conflicts**, which occur when two branches contain file changes that cannot be merged with the git merge command. Step 5 involves git push.

### git push
Now it's time to share your work with the rest of the world.
The command: `git push origin your_branch_name` will push your branch up to the remote, origin. From there, Sally can review your branch and merge your work into the master branch, making it part of the definitive project version.

**Example:**  
```
$ git branch
* bio-questions
  master

$ git status
On branch bio-questions
nothing to commit, working directory clean

$ git push origin bio-questions
Counting objects: 3, done.
Delta compression using up to 16 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 404 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
To /home/ccuser/workspace/curriculum-a/science-quizzes
 * [new branch]      bio-questions -> bio-questions
 ```
In the output, notice the line:
```
To /home/ccuser/workspace/curriculum/science-quizzes
 * [new branch]      bio-questions -> bio-questions
 ```
Git informs us that the branch bio-questions was pushed up to the remote. Sally can now review your new work and can merge it into the remote’s master branch.

### generalization

Now you know enough to start collaborating on Git projects! Let’s review.

* A `remote` is a Git repository that lives outside your Git project folder. Remotes can live on the web, on a shared network or even in a separate folder on your local computer.
* **The Git Collaborative Workflow are steps that enable smooth project development when multiple collaborators are working on the same Git project.**

We also learned the following commands:

* `git clone`: Creates a local copy of a remote.
* `git remote -v`: Lists a Git project’s remotes.
* `git fetch`: Fetches work from the remote into the local copy.
* `git merge origin/master`: Merges origin/master into your local branch.
* `git push origin <branch_name>`: Pushes a local branch to the origin remote.

Git projects are usually managed on Github, a website that hosts Git projects for millions of users. With Github you can access your projects from anywhere in the world by using the basic workflow you learned here. OR if you host your own source code management like gerrit on PREMISES your project team or repository manager or configuration management team might have rules to disallow direct push onto master. Complement these new knowledges with your local best practice. 

### javascrip project



## TODO - Moving git repository with full history: </br>
https://www.atlassian.com/git/tutorials/git-move-repository </br>
https://gist.github.com/niksumeiko/8972566 </br>
https://medium.com/@zeebaig/how-to-migrate-git-repository-with-branches-and-commit-history-dd129fd36dca </br>
https://medium.com/@ayushya/move-directory-from-one-repository-to-another-preserving-git-history-d210fa049d4b </br>


## TODO - Configure nginx redirect with docker-compose to handle if someone forgot to configure new remote repo url: </br>


## GERRITCODEREVIEW

**Great Links** </br>
Documentation: https://gerrit-documentation.storage.googleapis.com/Documentation/2.16.8/index.html </br>
Plugins: https://gerrit-documentation.storage.googleapis.com/Documentation/2.16.8/config-plugins.html </br>
Tutorial: https://www.mediawiki.org/wiki/Gerrit/Tutorial#Set_Up_SSH_Keys_in_Gerrit </br>
Gerrit in production: https://github.com/GerritCodeReview/docker-gerrit </br>
</br>
**Gerrit plug-n-play in docker** </br>
Quickly gets gerrit up n running to test it out: 
```
docker run -ti -p 8080:8080 -p 29418:29418 gerritcodereview/gerrit
```

**Gerrit ssh key n ssh to Gerrit** </br>
Step 1: ssh keygen
```
ssh-keygen -t rsa -C "iam.external@wellwellvell.com"
```

Step 2: get ssh up and running, add private key to agent and check gerrit version through ssh daemon
```
eval `ssh-agent`
ssh-add .ssh/id_rsa
ssh -p 29418 admin@localhost gerrit version
```

**Gerrit plugins** </br>
Possible to install plugins via plugins manager. Just click install and plugin will be downloaded and installed in the plugins directory. Suitable candidate is messageoftheday plugin.

List all plugins: 
```
ssh -p 29418 admin@localhost gerrit plugin ls
```

List all plugins - output:
```
Name                           Version    Status   File
-------------------------------------------------------------------------------
avatars-gravatar               381fc84a89 ENABLED  avatars-gravatar.jar
codemirror-editor              v2.16.8    ENABLED  codemirror-editor.jar
commit-message-length-validator v2.16.8    ENABLED  commit-message-length-validator.jar
delete-project                 v2.16-116-g6787290183 ENABLED  delete-project.jar
download-commands              v2.16.8    ENABLED  download-commands.jar
events-log                     v2.13-284-g7c88d05a87 ENABLED  events-log.jar
find-owners                    a11833e109 ENABLED  find-owners.jar
gitiles                        2c91cbc413 ENABLED  gitiles.jar
healthcheck                    0dd5329ea2 ENABLED  healthcheck.jar
heartbeat                      0371a82791 ENABLED  heartbeat.jar
hooks                          v2.16.8    ENABLED  hooks.jar
messageoftheday                3f65e8bfc6 ENABLED  messageoftheday.jar
plugin-manager                 v2.15.3-6-g613e25e1b7 ENABLED  plugin-manager.jar
replication                    v2.16.8    ENABLED  replication.jar
reviewnotes                    v2.16.8    ENABLED  reviewnotes.jar
singleusergroup                v2.16.8    ENABLED  singleusergroup.jar
uploadvalidator                f7b1c59e78 ENABLED  uploadvalidator.jar
```

Reload plugin after installing - messageoftheday:
```
ssh -p 29418 admin@localhost gerrit plugin reload messageoftheday
```

Install heartbeat.jar plugin: </br>
```
ssh -p 29418 admin@localhost gerrit plugin install https://gerrit-ci.gerritforge.com/view/Plugins-stable-2.16/job/plugin-heartbeat-bazel-stable-2.16/lastSuccessfulBuild/artifact/bazel-bin/plugins/heartbeat/heartbeat.jar
```

Install heartbeat.jar - tail log - output: </br>
```
[2020-01-29 23:04:22,852] [SSH gerrit plugin install https://gerrit-ci.gerritforge.com/view/Plugins-stable-2.16/job/plugin-heartbeat-bazel-stable-2.16/lastSuccessfulBuild/artifact/bazel-bin/plugins/heartbeat/heartbeat.jar (admin)] INFO  com.google.gerrit.server.plugins.PluginLoader : Installed plugin heartbeat
[2020-01-29 23:05:15,910] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin heartbeat
[2020-01-29 23:05:16,025] [PluginScanner] INFO  com.ericsson.gerrit.plugins.heartbeat.HeartbeatDaemon : Initialized to send heartbeat event every 15000 milliseconds
[2020-01-29 23:05:16,043] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin heartbeat, version 0371a82791
[2020-01-29 23:05:16,043] [PluginScanner] INFO  com.ericsson.gerrit.plugins.heartbeat.HeartbeatDaemon : Stopped sending heartbeat event
[2020-01-29 23:05:16,045] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin heartbeat, version 0371a82791
[2020-01-29 23:06:16,624] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.CleanupHandle : Cleaned plugin plugin_heartbeat_200129_2304_303494816642575780.jar
```

Install healthcheck.jar plugin: </br>
```
ssh -p 29418 admin@localhost gerrit plugin install https://gerrit-ci.gerritforge.com/view/Plugins-stable-2.16/job/plugin-healthcheck-bazel-stable-2.16/lastSuccessfulBuild/artifact/bazel-bin/plugins/healthcheck/healthcheck.jar
```

Install healthcheck.jar - tail log - output: </br>
```
[2020-01-29 23:00:28,240] [SSH gerrit plugin install https://gerrit-ci.gerritforge.com/view/Plugins-stable-2.16/job/plugin-healthcheck-bazel-stable-2.16/lastSuccessfulBuild/artifact/bazel-bin/plugins/healthcheck/healthcheck.jar (admin)] INFO  com.google.gerrit.server.plugins.PluginLoader : Installed plugin healthcheck
[2020-01-29 23:01:15,958] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin healthcheck
Jan 29, 2020 11:01:16 PM com.google.inject.servlet.GuiceFilter setPipeline
WARNING: Multiple Servlet injectors detected. This is a warning indicating that you have more than one GuiceFilter running in your web application. If this is deliberate, you may safely ignore this message. If this is NOT deliberate however, your application may not work as expected.
[2020-01-29 23:01:16,129] [PluginScanner] INFO  com.google.gerrit.server.config.PluginConfigFactory : No /var/gerrit/etc/healthcheck.config; assuming defaults
[2020-01-29 23:01:16,140] [PluginScanner] WARN  com.google.gerrit.server.plugins.PluginLoader : Cannot load plugin healthcheck
java.lang.IllegalArgumentException: A metric named plugins/healthcheck/reviewdb/latest_latency already exists
	at com.codahale.metrics.MetricRegistry.register(MetricRegistry.java:97)
	at com.google.gerrit.metrics.dropwizard.CallbackMetricImpl0.register(CallbackMetricImpl0.java:72)
	at com.google.gerrit.metrics.dropwizard.DropWizardMetricMaker.newTrigger(DropWizardMetricMaker.java:304)
	at com.google.gerrit.server.plugins.PluginMetricMaker.newTrigger(PluginMetricMaker.java:161)
	at com.google.gerrit.metrics.MetricMaker.newTrigger(MetricMaker.java:139)
	at com.googlesource.gerrit.plugins.healthcheck.HealthCheckMetrics.start(HealthCheckMetrics.java:80)
	at com.google.gerrit.lifecycle.LifecycleManager.start(LifecycleManager.java:95)
	at com.google.gerrit.server.plugins.ServerPlugin.startPlugin(ServerPlugin.java:246)
	at com.google.gerrit.server.plugins.ServerPlugin.start(ServerPlugin.java:175)
	at com.google.gerrit.server.plugins.PluginLoader.runPlugin(PluginLoader.java:495)
	at com.google.gerrit.server.plugins.PluginLoader.rescan(PluginLoader.java:423)
	at com.google.gerrit.server.plugins.PluginScannerThread.run(PluginScannerThread.java:42)
```

Install its-jira.jar plugin: </br>
```
ssh -p 29418 admin@localhost gerrit plugin install -n its-jira.jar https://gerrit-ci.gerritforge.com/view/Plugins-stable-2.16/job/plugin-its-jira-bazel-stable-2.16/lastSuccessfulBuild/artifact/bazel-bin/plugins/its-jira/its-jira.jar
```

Install its-jira plugin - tail log: </br>
```
docker logs 1b7ee3d03e4a
```

Install its-jira plugin - tail log - output: </br>
```
com.google.gerrit.server.plugins.PluginInstallException: No valid Jira server configuration was found for project 'All-Projects'
.Missing one or more configuration values: url: null, username: null, password: null
	at com.google.gerrit.server.plugins.PluginLoader.runPlugin(PluginLoader.java:516)
	at com.google.gerrit.server.plugins.PluginLoader.installPluginFromStream(PluginLoader.java:190)
	at com.google.gerrit.sshd.commands.PluginInstallCommand.doRun(PluginInstallCommand.java:85)
	at com.google.gerrit.sshd.commands.PluginAdminSshCommand.run(PluginAdminSshCommand.java:34)
	at com.google.gerrit.sshd.SshCommand$1.run(SshCommand.java:44)
	at com.google.gerrit.sshd.BaseCommand$TaskThunk.run(BaseCommand.java:463)
	at com.google.gerrit.server.logging.LoggingContextAwareRunnable.run(LoggingContextAwareRunnable.java:83)
	at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
	at java.util.concurrent.FutureTask.run(FutureTask.java:266)
	at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.access$201(ScheduledThreadPoolExecutor.java:180)
	at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:293)
	at com.google.gerrit.server.git.WorkQueue$Task.run(WorkQueue.java:646)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
	at java.lang.Thread.run(Thread.java:748)
Caused by: java.lang.RuntimeException: No valid Jira server configuration was found for project 'All-Projects'
.Missing one or more configuration values: url: null, username: null, password: null
	at com.googlesource.gerrit.plugins.its.jira.JiraItsServer.getFacade(JiraItsServer.java:60)
	at com.googlesource.gerrit.plugins.its.jira.JiraItsStartupHealthcheck.start(JiraItsStartupHealthcheck.java:43)
	at com.google.gerrit.lifecycle.LifecycleManager.start(LifecycleManager.java:95)
	at com.google.gerrit.server.plugins.ServerPlugin.startPlugin(ServerPlugin.java:246)
	at com.google.gerrit.server.plugins.ServerPlugin.start(ServerPlugin.java:175)
	at com.google.gerrit.server.plugins.PluginLoader.runPlugin(PluginLoader.java:495)
	... 14 more
fatal: Plugin failed to install. Cause: No valid Jira server configuration was found for project 'All-Projects'
.Missing one or more configuration values: url: null, username: null, password: null


[2020-01-29 22:47:48,409] [SSH gerrit plugin install -n its-jira.jar https://gerrit-ci.gerritforge.com/view/Plugins-stable-2.16/job/plugin-its-jira-bazel-stable-2.16/lastSuccessfulBuild/artifact/bazel-bin/plugins/its-jira/its-jira.jar (admin)] WARN  com.google.gerrit.server.plugins.PluginLoader : Plugin provides its own name: <its-jira>, use it instead of the input name: <its-jira.jar>
[2020-01-29 22:47:48,597] [SSH gerrit plugin install -n its-jira.jar https://gerrit-ci.gerritforge.com/view/Plugins-stable-2.16/job/plugin-its-jira-bazel-stable-2.16/lastSuccessfulBuild/artifact/bazel-bin/plugins/its-jira/its-jira.jar (admin)] INFO  com.googlesource.gerrit.plugins.its.jira.JiraModule : JIRA is configured as ITS
```
