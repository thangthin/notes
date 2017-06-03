# Collaboration Setup
As a lone developer, you're probably comfortable working with a local repository. In this first lesson, we're going to talk about **remote repositories** and interacting with these remote repositories.

Let's say that you have a friend, we'll call her Farrin, and one day you two were together and you showed her what you've been working on. She had some ideas on features she could contribute to the project. But you don't want to give her your computer for her to make these changes, you want her to work on her computer. And, you don't want to have to wait for her to add these features, you want to keep working on the project and then just merge in her changes with she's finished. So how can we do that?

Well, let me tell you that emailing the project back and forth would be a maintenance nightmare after about two emails. You're already tracking your project with Git, so we'll use it to manage everything.

So Farrin will work on the project on a specific branch and any changes she makes she'll add to that branch. While she's working in her branch, you'll work on the project but on your own specific branch. And then you can merge these branches together when you get the branch from Farrin.

> 💡 ## Always Use Topic Branches
> Remember that it's incredibly helpful to make all of your commits on descriptively named topic branches.  Branches help isolate unrelated changes from each other.
> 
> So when you're collaborating with other developers make sure to create a new branch that has a descriptive name that describes what changes it contains.

## git push vs git pull
if you have changes in your local repository that you want in your remote repository, you'll ```git push```
vice versa ```git pull``` grabs the changes from remote onto your local repository.

# What is a Remote Repository?
Git is a distributed version control system which means there is not one main repository of information. Each developer has a copy of the repository. So you can have a copy of the repository (which includes the published commits and version history) and your friend can also have a copy of the same repository. Each repository has the exact same information that the other ones have, there's no one repository that's the main one.

Up until this point, you have probably been only working locally on a local repository. A remote repository is the same Git repository like yours but it exists somewhere else.

![git remote repos](git_img/ud456-l1-02-local-and-remote-repos.png)

# Ways to access a Remote
Remotes can be accessed in a couple of ways:

- with a URL
- path to a file system
Even though it's possible to create a remote repository on your file system, it's very rarely used. By far the most common way to access a remote repository is through a URL to a repository that’s out on the web.

The way we can interact and control a remote repository is through the Git remote command:
```
$ git remote
```

## Branches on local to remote
We can merge local **topic branch** onto ```master``` branch and push to remote as a ```master``` branch
or we can keep track the branches separately on the remote repository as well.

## Why Multiple Remotes?
Why would you want to have multiple remote repositories? We'll look at this later but briefly, if you are working with multiple developers then you might want to get changes they're working on in their branch(es) into your project before they merge them into the master branch. You might want to do this if you want to test out their change before you decide to implement your changes.

Another example is if you have a project whose code is hosted on Github but deploys via Git to Heroku. You would have one remote for the ```master``` and one for the ```deployment```.

# The Git Remote Command
The git remote command will let you manage and interact with remote repositories.
```
$ git remote
```
Try running this command on a local repository that you haven't shared with anyone yet. What do you get?
![git remote](git_img/ud456-l1-git-remote-no-remote.png)

If you haven't configured a remote repository then this command will display nothing. One caveat to this is if you have cloned a repository. If you have, then your repository will automatically have a remote because it was cloned from the repository at the URL you provided. Let's look at a repository that has been cloned.
![git remote of cloned repo](git_img/ud456-l1-git-remote-shortname.png)

## Remote Shortnames
The output of ```git remote``` is just the word ```origin```. Well that's weird. The word "origin", here, is referred to as a "shortname". A shortname is just a short and easy way to refer to the location of the remote repository. A shortname is local to the current repository (as in, your local repository). The word "origin" is the defacto name that's used to refer to the main remote repository. It's possible to rename this to something else, but typically it's left as "origin".

Why do we care about how easy it is to refer to a remote repositories location? Well as you'll soon find out we'll be needing the path to the remote repository in a lot of our commands. And it's a lot easier to use just a name rather than the entire path to the remote repository.

For example which one of these is easier to understand:

- Head north for about a quarter of a mile, then turn left, go straight down that road for about 5 miles, then turn right, proceed straight for about 300 feet until you past the blue mailbox, turn left down Jack Street, go 50 feet then turn left again on Owen Road, that will curve around until you hit Finn Lane. The structure that's the third one on the left
- Grandma's house
You can see that it's a lot easier to refer to a location by just a short name like Grandma's house rather than the entire way to get there from your current location 😉

If you want to see the full path to the remote repository, then all you have to do is use the ```-v``` flag:
![git remote -v](git_img/ud456-l1-git-remote-from-clone.png)
The Terminal application running the ```git remote``` command. The output includes the shortname and the full URL that it refers to.
Here you can see that if the word ```origin``` is used, what actually is used is the path to ```https://github.com/GoogleChrome/lighthouse.git```. It also might seem a little bit odd that there are now two remotes both of them "origin" and both going to the same URL. The only difference is right at the end: the (fetch) part and the (push) part

We'll be looking at both fetch and push in upcoming sections.

## Adding a remote repo to an existing repo
```
$ git remote add origin <url to your remote repo>
```

```git remote add``` was used to create a **shortname** of ```origin``` that points to the project on GitHub. Running ```git remote -v``` displays both the shortname and the URL.

## Recap
A remote repository is a repository that's just like the one you're using but it's just stored at a different location. To manage a remote repository, use the ```git remote``` command:
```
$ git remote
```
- It's possible to have links to multiple different remote repositories.
- A shortname is the name that's used to refer to a remote repository's location. Typically the location is a URL, but it could be a file path on the same computer.
- ```git remote add``` is used to add a connection to a new remote repository.
- ```git remote -v``` is used to see the details about a connection to a remote.

# Sending Commits
To send local commits to a remote repository you need to use the ```git push``` command. You provide the remote short name and then you supply the name of the branch that contains the commits you want to push:
```
$ git push <remote-shortname> <branch>
```
My remote's shortname is ```origin``` and the commits that I want to push are on the ```master``` branch. So I'll use the following command to send my commits to the remote repository on GitHub:
```
$ git push origin master
```
Run the following command:
```
$ git log --oneline --graph --decorate --all
```
We now have a new marker in the output! This marker is ```origin/master``` and is called a **tracking branch**. A tracking branch's name includes the shortname of the remote repository as well as the name of the branch. So the tracking branch ```origin/master``` is telling us that the remote ```origin``` has a ```master``` branch that points to commit ```9b7d28f``` (and includes all of the commits before ```9b7d28f```). This is really helpful because this means we can track the information of the remote Repository right here in our local one!

One very important thing to know is that this ```origin/master``` tracking branch is not a live representation of where the branch exists on the remote repository. If a change is made to the remote repository not by us but by someone else, the ```origin/master``` tracking branch in our local repository will not move. We have to tell it to go check for any updates and then it will move. We'll look at how to do this in the next section.

## Recap
The ```git push``` command is used to send commits from a local repository to a remote repository.
```
$ git push origin master
```
The ```git push``` command takes:

- the shortname of the remote repository you want to send commits to
- the name of the branch that has the commits you want to send

# Pull changes from a remote
Let’s say that we are in a situation where there are commits on the remote repository that we do not have in our local repository. This can happen in several ways: You could be working on a team, and a co-worker has pushed new changes to the remote. Alternatively, you could be working on the same project but from different computers -- for example, say you have a work computer and a personal computer, and you contribute to the repo from both of them. If you push changes to the repo from your work computer, the local repo on your personal computer will not reflect those changes. How do we sync new changes that are on the remote into the local repository? That's exactly where we're going to be looking at now. Let's first look at how pulling in remote changes works in theory, then we'll actually do it ourselves!

I said it before but I'll say it again, the branch that appears in the local repository is actually **tracking a branch** in the remote repository (e.g. ```origin/master``` in the local repository is called a **tracking branch** because it's tracking the progress of the master branch on the remote repository that has the shortname "origin").

## Retrieve remote commits
```
$ git pull origin master
```
```git push``` will sync the remote repository with the local repository. To do with the opposite (to sync the local with the remote), we need to use ```git pull```. The format for ```git pull``` is very similar to ```git push``` - you provided the shortname for the remote repository and then the name of the branch you want to pull in the commits.

If you don't want to automatically merge the local branch with the tracking branch then you wouldn't use ```git pull``` you would use a different command called ```git fetch```. You might want to do this if there are commits on the repository that you don't have but there are also commits on the local repository that the remote one doesn't have either.

## Recap
If there are changes in a remote repository that you'd like to include in your local repository, then you want to pull in those changes. To do that with Git, you'd use the git pull command. You tell Git the shortname of the remote you want to get the changes from and then the branch that has the changes you want:
```
$ git pull origin master
```
When ```git pull``` is run, the following things happen:

- the commit(s) on the remote branch are copied to the local repository
- the local tracking branch (```origin/master```) is moved to point to the most recent commit
- the local tracking branch (```origin/master```) is merged into the local branch (```master```)
Also, changes can be manually added on GitHub (but this is not recommended, so don't do it).

# Git Fetch vs Git Pull
Git fetch is used to retrieve commits from a remote repository's branch but it does not automatically merge the local branch with the remote tracking branch after those commits have been received.

The above paragraph is a little dense so why don't you reread it one more time.

You provide the exact same information to ```git fetch``` as you do for ```git pull```. So you provide the shortname of the remote repository you want to fetch from and then the branch you want to fetch:
```
$ git fetch origin master
```
**Note:** When we add remote, we're really creating a new remote/branch (e.g ```origin/master```) tracking the branch.
So when we do git fetch, it pulls the commit for the remote tracking branch e.g ```origing/master``` but not for the ```master``` branch itself.

When ```git fetch``` is run, the following things happen:

- the commit(s) on the remote branch are copied to the local repository
- the local tracking branch (e.g. ```origin/master```) is moved to point to the most recent commit
The important thing to note is that the local branch does not change at all.

You can think of ```git fetch``` as half of a ```git pull```. The other half of ```git pull``` is the merging aspect.

One main point when you want to use ```git fetch``` rather than ```git pull``` is if your remote branch and your local branch both have changes that neither of the other ones has. In this case, you want to fetch the remote changes to get them in your local branch and then perform a merge manually. Then you can push that new merge commit back to the remote.

## Recap
You can think of the ```git pull``` command as doing two things:

- fetching remote changes (which adds the commits to the local repository and moves the tracking branch to point to them)
- merging the local branch with the tracking branch
The ```git fetch``` command is just the first step. It just retrieves the commits and moves the tracking branch. It does not merge the local branch with the **tracking branch**. The same information provided to ```git pull``` is passed to ```git fetch```:

- the shorname of the remote repository
- the branch with commits to retrieve
```
$ git fetch origin master
```