## Git Tutorial for the PCT Case Studies
You probably heard of Git before, but it's possible that you haven't used it. Writing a case study doesn't require masterful Git skills, but you will need a few basics. This small tutorial will go over all the steps, taking you from zero to case study.

### Get Git
If you don't have Git installed, head over to the official [Git Download page and download it](https://git-scm.com/download/win). Once installed and downloaded, you might also want to install [Posh Git](https://github.com/dahlbyk/posh-git). If you're already using Chocolatey or Windows 10's package manager to install software, you can simply run the following command from an elevated PowerShell (right click, select 'Run as Administrator'):

```
cinst git.install
cinst poshgit

# Restart PowerShell / CMDer before moving on - or run
$env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine") + ";" + [System.Environment]::GetEnvironmentVariable("Path","User") 

cinst git-credential-winstore
cinst github
```

##### A Word About Git Clients
Many users are initially put off by the idea of having to work with Git through the command line. I distinctly remember not even considering the PowerShell/Terminal and instead downloading one of the man GUI tools that are available. As someone who went through the experience of trying to use a Git GUI without really understanding what Git is doing on the command-line-level, let me tell you: It will end in tears.

In fact, I find it a lot easier today to work with the command-line. Git will do *exactly* what you tell it do - each step will be obvious to you. GUIs often try to combine multiple commands together into one fancy button, which will surely blow your whole project up, if you don't understand what's going on under the hood. For those reasons, I *heavily* recommend sticking with the command-line.

### Fork the Repository to your Account
Before we can get started, you need to register with GitHub. Either create or login into your account. Then, head over to the [catalystcode/case-studies](https://github.com/CatalystCode/case-studies) repository and click the little 'fork' button in the upper right.

![Fork the repo](https://raw.githubusercontent.com/CatalystCode/case-studies/gh-pages/tutorial/fork.png)

This will create a copy of the repository as it exists in the `catalystcode` organization (hence called catalystcode/case-studies) in your own account. In my case, I'm controlling both the original (also called "upstream") repository in `catalystcode` as well as in my own account, found at `felixrieseberg/case-studies`. 

### Clone the Case Study Repository to your Machine
Visit your fork (which should be at github.com/{your_name}/case-studies) and copy the "HTTPS Clone URL". Using this URL, you're able to `clone` the repository, which is really just a fancy way of saying "download the whole repository, including its history and information about its origin".

![Clone the repo](https://raw.githubusercontent.com/CatalystCode/case-studies/gh-pages/tutorial/clone.png)

Now that you have Git installed, open up PowerShell. If everything worked correctly, you should be able to run `git --version`. If that works, navigate to a folder where you'd like to keep the `case-studies` repository (and hence, all your case studies). To get a copy of your fork onto your local machine, run:

```
git clone https://github.com/{YOUR_USERNAME}/case-studies
```

This should generate output that looks roughly like this:

```
Windows PowerShell
Copyright (C) 2015 Microsoft Corporation. All rights reserved.

C:\Users\feriese> git --version
git version 1.9.5.msysgit.1
C:\Users\feriese> git clone https://github.com/felixrieseberg/case-studies
Cloning into 'case-studies'...
remote: Counting objects: 1027, done.
remote: Compressing objects: 100% (4/4), done.
Receiving objects: 100% (1027/1027), 16.02 MiB | 9.11 MiB/s, done.
emote: Total 1027 (delta 0), reused 0 (delta 0), pack-reused 1023R
Resolving deltas:   5% (30/531)
Resolving deltas: 100% (531/531), done.
Checking connectivity... done.
C:\Users\feriese>
```

You can run `explorer case-studies` to open up the folder. All the files are there - including the history of the whole repository. The connection to your fork (`your_name/case-studies`) is still there. To confirm that, `cd` into the cloned folder and run the following command, which will show you all the remote repositories registered:

```
git remote -v
```

The output should be:

```
C:\Users\feriese\case-studies [gh-pages]> git remote -v
origin  https://github.com/felixrieseberg/case-studies (fetch)
origin  https://github.com/felixrieseberg/case-studies (push)
```

As you can see, we're connected to your fork of the case-studies (called "origin"), but currently not connected to the upstream version living in `catalystcode/case-studies`. To change that, we can simply add remotes. Run the following command:

```
git remote add upstream https://github.com/CatalystCode/case-studies
```

Entering `git remote -v` again should give you both repositories - both yours (called "origin") and the one for the whole team (called "upstream").

### Creating a new Branch for your Case Study
In modern Git development, every single change that you want to make to the code base will be made in a "branch". Like a tree branch, the branch is "based" on a different branch. In our case, your base branch will always be `gh-pages`, which is the default branch name for GitHub pages. In order to create a new branch, you can always run:

```
# This makes sure that your new branch is based on gh-pages
git checkout gh-pages
# This creates the new branch
git checkout -b my-new-branch 
```
You can now go ahead and make your changes - adding files, writing case studies, fixing typos. Keep in mind that a branch should host isolated changes: You would normally not fix someone else's case study and add a new one in branch. Instead, you create one branch that fixes typos; and another branch for your added case study.

### Staging your Changes for a Commit
Now that you made your changes, you can "stage" them for a commit. Whenever you stage a file for a commit, you make a snapshot of the file at the time you're staging it for a commit. If you change a file after you staged it, you will have to stage it again. To stage a file, simply run:

```
git add ./path-to/my-file.md
```

If you just want to stage all files in your current repository (including deletions), run:

```
git add --all .
```

### Commit your Changes
Now that your changes have been staged, we're ready to commit them. You can either pass the commit command a title for your commit - or omit the parameter, in which case Git will open up the default text editor to create a commit message.

###### Hint: Changing the Default Editor
The default editor will most likely be Vim. If you don't like that, use [GitPad](https://github.com/github/GitPad) - it'll make sure that you can edit all your Git messages with whatever editor is associated with `txt` files.

To commit the quick way:
```
git commit -m "Add Case Study: How Git is annoying"
```

To commit the long way, allowing you to define both title and message of your commit:
```
git commit
```

### Push your new Branch to Your Fork on GitHub
You wrote a case study, made some changes, committed - now we have to make sure that your changes also end up on GitHub. To do so, we have to push your local branch to your fork on GitHub. Run the command below (obviously using the name of the branch you want to push)

```
git checkout NAME_OF_YOUR_BRANCH
git push -u origin NAME_OF_YOUR_BRANCH
```

### Make a Pull Request
Now, head over to the [catalystcode/case-studies](https://github.com/CatalystCode/case-studies) repository. In most cases, GitHub will pick up on the fact that you just pushed a branch with some changes to your local fork, assuming that you probably want for those changes to end up in the upstream branch. To help you, it'll show a little box right above the code asking you if you want to make a pull request.

![Make a Pull Request](https://raw.githubusercontent.com/CatalystCode/case-studies/gh-pages/tutorial/pullrequest.png)

Click that button. GitHub will open up the 'Create a Pull Request Page'. It is probably a good idea to notify potential reviewers in your pull request. Be sure to @mention (at least) two reviewers: (1) a domain expert in the topic you cover, and (2) an engineer who is not familiar with the topic.

As soon as you hit the 'Create Pull Request' button, it'll show up in the list of pull requests and the bots will take over.

### Submitting feedback on a Pull Request
Once your case study Pull Request is available for all to see, people can comment on it. This is easily done directly in github, while viewing the Pull Request. Navigate to the `Commits` tab, and select the commit to comment on:

![Viewing commits](./tutorial/commitlist.png)

As you scroll through the added text, you'll see a `+` symbol floating atop each paragraph. Just click it to add a comment:

![Click + to add comment](./tutorial/addcomment.png)

![Enter comment text](./tutorial/entercomment.png)

The author can now review these comments and make updates as appropriate, possibly via an updated commit+PR.

### Bonus: Squashing Commits
You should commit often. It's a great backup and safety net in case you mess up. At the same time, you don't want your pull request to contain all your commits - in practice, a pull request should contain one commit. There are obvious exceptions to this rule (like epic projects - think 'Windows support for Docker'), but your little case study should most definitely be just one commit. For that to happen, we need to rewrite history.

Rewriting Git history is a little bit scary, but it's easy to do. Here's the setup: You just made six commits to your branch. Before making a pull request, you want all those commits to be turned into one. To change the history of your last six commits, run:

```
git rebase -i HEAD~6
```

It is important that you change only commits that *you* made, since your branch will not be compatible with "upstream" (catalystcode/case-studies) if you rewrite history that is already present in the repository there. You can however mess with your own fork as much as you want to, since you should be the only person working with it.

Once you run the command, you will be presented with a screen that looks like this:

```
pick 4a67852 Update interactions.js
pick e9c1c12 Further Mini-JS Optimizations
pick 676adb6 Listener is always undefined
pick 6e8ba27 Automated Jekyll Deployment
pick fc0c120 Remove Gemfile Lock
pick 1cd833d Fix Deployment Folder
```

See how every single commit begins with the word `pick`, followed by its commit id and the title? You can replace the `pick` with an action of your choice. Available are:

 * `p`, `pick`: use commit
 * `r`, `reword`: use commit, but edit the commit message
 * `e`, `edit`: use commit, but stop for amending
 * `s`, `squash`: use commit, but meld into previous commit
 * `f`, `fixup`: like "squash", but discard this commit's log message
 * `x`, `exec`: run command (the rest of the line) using shell

In order to squash all six commits into the very first one you made, you would change the file to:

```
pick 4a67852 Update interactions.js
fixup e9c1c12 Further Mini-JS Optimizations
fixup 676adb6 Listener is always undefined
fixup 6e8ba27 Automated Jekyll Deployment
fixup fc0c120 Remove Gemfile Lock
fixup 1cd833d Fix Deployment Folder
```

Then, once you're done, update the branch on your own fork in GitHub. Push with the `f` parameter (force), telling Git to overwrite whatever GitHub has in the branch:

```
git push -f
```

If you never pushed the branch before, you can use the normal push command - there's no history that we need to overwrite with the `f` paramter.

```
git push -u origin NAME_OF_YOUR_BRANCH
```