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