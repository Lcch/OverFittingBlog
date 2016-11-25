---
title:  "GitHub Basic"
date:   2015-08-29 17:10:02
category: Computer Technology
---

## What is Git and GitHub?

Git is a version control tool. GitHub is a code sharing and collaboration platform. This post summaries some basic important Git commands and guides you how to manage project and code better.

<!--more-->

## Saving changes

![Lifecycle of the status of files](/images/lifecycle.png)

**git status** checks the status of files  

{% codeblock lang:bash %}
git status
{% endcodeblock %}

**git add** command proposes changes to staging area.

{% codeblock lang:bash %}
git add <file> // add a certain file
{% endcodeblock %}

Edit **.gitignore** file to list all files you don't want Git to automatically add or show you as being untraced.

**git diff** is to see what you've changed but not yet staged.

{% codeblock lang:bash %}
    git diff --staged // to see what you've staged that will go into next commit.
{% endcodeblock %}

Once you've already set up staging area, you can **commit your changes**. Any change that is unstaged won't go into this commit. 

{% codeblock lang:bash %}
    git commit  // you're able to confirm the commit and write down the description
    git commit -m "Description" // confirm and write down the descriptino directly
{% endcodeblock %}

**Removing** files: 

{% codeblock lang:bash %}
    git rm <file> // remove file from it from tracked filed. It will remove file from working directory as well.
    git rm --cached <file> // Same as above but will not remove file from working directory.
{% endcodeblock %}

**Moving** (Renaming) files:
    
{% codeblock lang:bash %}
    git mv <old_file> <new_file>
{% endcodeblock %}

<br>

## Undoing changing

**Recommit** with adding new changes. For example, when you forget to add some files but you don't want to make a new commit. You are able to use _git commit_ with _amend_ option.

{% codeblock lang:bash %}
    git commit --amend  // try to commit again with adding new changes
{% endcodeblock %}

**Ustaging** a staged file. When you add a file into staging area by mistake, you want to ustage the file. You can use _git reset HEAD_

{% codeblock lang:bash %}
    git reset <file>   // Ustaging file from staged area.
    git reset // unstages all files without overwriting any changes.
{% endcodeblock %}

**Unmodifing** a file. When you don't want to keep changes of a file. You want to revert it back as it looks like in latest commit. You can use following command. But be careful, once you use this command, any changes of the file you made after latest commit are gone.

{% codeblock lang:bash %}
    git checkout -- <file> // Unmodifing the file.
{% endcodeblock %}

**Viewing old commit** 

{% codeblock lang:bash %}
    git checkout <commit> <file> // update a single file in the working dierctory to match the specified commit
    git checkout <commit> // update all files in the working directory to match the specified commit.
    git checkout HEAD // check out back to the most recent version
{% endcodeblock %}

**Reverting** _git revert_ undoes a commited snapchat. Instead of removing the commite from the history, it undos the changes introduced by specified commit and appends a new commit. 

{% codeblock lang:bash %}
    git revert <commit>  // Generate a new commit that undoes all of the changes introduced in <commit>, then apply it to the current branch.
    git revert HEAD      // revert the commit we just created
{% endcodeblock %}


**Remove a commit**

{% codeblock lang:bash %}
    git reset <commit> // Move the current branch tip backward to <commit>, reset the staging area to match, 
                       // but leave the working directory alone. All changes made since <commit> will reside 
                       // in the working directory, which lets you re-commit the project history using cleaner, 
                       // more atomic snapshots.
    git reset --hard <commit> // Move the current branch tip backward to <commit> and reset both the staging 
                              // area and the working directory to match.
{% endcodeblock %}

**_WARNING_**: never perform the reset operation if you've already pushed your commits to a shared repository.

<br>

## Working with remote

**git remote** is to manager the remote

{% codeblock lang:bash %}
    git remote -v // shows all of remote
    git remote add <shortname> <url> // add remote url as a shortname you can reference easily
    git remote rm <shortname> // remove
    git remote rename <old-name> <new-name> // rename
{% endcodeblock %}

**git fetch** is to pull down all of data from the remote. After you do this, you should have references to all the branches from that remote, which you can merge in or inspect at any time.

{% codeblock lang:bash %}
    git fetch <remote> // Fetch all of the branches from the repository. This also downloads all of the required commits and files from the other repository.
    git fetch <remote> <branch> // Same as the above command, but only fetch the specified branch.
{% endcodeblock %}

**git push** once you finish the code or the files you want to share. You can use the command to push the project to remote.

{% codeblock lang:bash %}
    git push <remote-shortname> <branch-name>
    git push <remote-shortname> --all // push all of your local branches. 
{% endcodeblock %}

**git pull** 

{% codeblock lang:bash %}
    git pull <remote> // Fetch the specified remote’s copy of the current branch and immediately merge it into the local copy. It's the same as git fetch and then git merge
    git pull --rebase <remote> // Same as the above command, but instead of using git merge to integrate the remote branch with the local one, use git rebase.
{% endcodeblock %}

<br>

## Using branches

A branch represents an independent line of development. Any changes of a branch will not affect other branches. Because of this, you can develope diffrent features in the same time, but will not mess up the code at all. 

{% codeblock lang:bash %}
    git branch // List all of the brances in your repository.
    git branch <branch> // create a branch based on current branch
    git branch -d <branch> // delete the specified branch
    git checkout <existing-branch> // switch current branch to another branch
{% endcodeblock %}

![Branch Example](/images/branch.svg)

For instance, the above image shows 3 in dependent branches. In each branch, you can do anything you want like edit, commit, etc, but will not affect any other branches.

**git merge** it lets you take the independent lines of development created by git branch and integrate them into a single branch.

{% codeblock lang:bash %}
    git merge <branch> // Merge the specified branch into the current branch. Git will determine the merge algorithm automatically.
{% endcodeblock %}

If the two brancehs you're trying to merge both changed the same part of the same file, it stopes right before the merge commit so that you have to resolve the conflicts manually.

<br>

## How it works

### Scenario 1: 

For example, A and B are working together on a branch (e.g. master). A, B develop in their own local repository. Once A finishes his development, he want to publish his local commits to the central (remote, shared) repository. He can do this with following command:

{% codeblock lang:bash %}
    git push upsteam master // Here we suppose upstream is the remote-shortname of the central repository
{% endcodeblock %}

Until now, everything goes well. After A published to central repository, B want to publish his local commits as well. B will use the same command to try to push to upstream. However, B's local history has diverged from the central repostiory. B will receive a refuse message. So B need to rebase his local working diretory by following repostiory.
    
{% codeblock lang:bash %}
    git pull --rebase upstream master 
{% endcodeblock %}

B may need to resolve conflicts during rebase. After B's done synchronizing with the central repository, B is able to publish his changes by _git push upstream master_.


### Scenario 2:

Only use master branch is a bad idea. Because it's hard to manage the project while the project is bigger and more complex. For instance, lots of people working on different features on master branch. The code will definitly mess up. In this case, using feature branch is an easy way to encourage collaboration.

For example, A is starting developing a new feature. A can request a new branch with following command in his local git to work on.

{% codeblock lang:bash %}
    git checkout -b new_feature master // suppose A create a new branch named new_feature based on master
{% endcodeblock %}

After a few commits, he finished the feature and push his feature branch up to the central (remote) repository. 

{% codeblock lang:bash %}
    git push -u upstream new_feature // Here we suppose upstream is the remote-shortname of the central repository
{% endcodeblock %}

Then A is able to file a pull request. Team members will do the code review. Once team members accept the pull request, someone needs to merge the feature into the stable project (like master branch).

{% codeblock lang:bash %}
    git checkout master
    git pull
    git pull upstream new_feature
    git push
{% endcodeblock %}

## Reference
[Git Tutorial of Atlassian](https://www.atlassian.com/git/tutorials)  
[Git-Scm](https://git-scm.com/)






