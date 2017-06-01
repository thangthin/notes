# Why Clone?
First, what is cloning?

> to make an identical copy

What's the value of creating an identical copy of something, and how does this relate to Git and version control?

Why would you want to create an identical copy? Well, when I work on a new web project, I do the same set of steps:

- create an ```index.html``` file
- create a ```js``` directory
- create a ```css``` directory
- create an ```img``` directory
- create ```app.css``` in the css directory
- create ```app.js``` in the js directory
- add starter HTML code in ```index.html```
- add configuration files for linting (validating code syntax)
  - [HTML linting](http://htmlhint.com/)
  - [CSS linting](https://stylelint.io/)
  - [JavaScript linting](http://eslint.org/)
- [configure my code editor](http://editorconfig.org/)

...and I do this every time I create a new project!...which is a lot of effort I'm putting in for each new project. I didn't want to keep doing these same steps over and over, so I did all of the steps listed above one last time and created a starter project for myself. Now when I create a new project, I just make an identical copy of that starter project!

The way that cloning relates to Git is that the command we'll be running on the terminal is ```git clone```. You pass a path (usually a URL) of the Git repository you want to clone to the ```git clone``` command.

Wanna try cloning an existing project? Let's see how Git's ```clone``` command works!

> # Verify Terminal Location
> TIP: Now before you clone anything, make sure you are located in the correct directory on the command line. Cloning a project creates a new directory and places the cloned Git repository in it. The problem is that you can't have nested Git repositories. So make sure the terminal's current working directory isn't located in a Git repository. If your current working directory is not in your shell's prompt, type ```pwd``` to print the working directory.

# Cloning The Blog Repository
Ready? Let's get cloning!

The command is ```git clone``` and then you pass the path to the Git repository that you want to clone. The project that we'll be using throughout this course is located at this URL: [https://github.com/udacity/course-git-blog-project]() So using this URL, the full command to clone blog project is:
```
$ git clone https://github.com/udacity/course-git-blog-project
```

# Git Clone Output Explanation
Let's look briefly at the output that ```git clone``` displays.

The first line says "Cloning into 'course-git-blog-project'...". Git is creating a directory (with the same name of the project we're cloning) and putting the repository in it...that's pretty cool!

The rest of the output is basically validation - it's counting the remote repository's number of objects, then it compresses and receives them, then it unpacks them.

# Clone Project And Use Different Name
You just cloned the blog project for this course. Awesome job!

The command you ran in the terminal was:
```
$ git clone https://github.com/udacity/course-git-blog-project
```
...which created a directory named ```course-git-blog-project```.

What if you want to use a different name instead of the default one? Yes, you could just run the command above and manually rename it in Finder/Windows Explorer or use ```mv``` on the terminal. But that's too many steps for us! Instead, we'd rather clone the project and have it use a different name all in one go! But how do we do that?

```
$ git clone https://github.com/udacity/course-git-blog-project blog-project
```

> # Not In A Git Repository?
> WARNING: Here's a very important step that often gets missed when you first start working with Git. When the ```git clone``` command is used to clone a repository, it creates a new directory for the repository...you already know this. But, it doesn't change your shell's working directory. It created the new repo inside the current working directory, which means that the current working directory is still outside of this new Git repo! Make sure you ```cd``` into the new repository.

> Remember to use the Terminal's command prompt as an aid - if you're in a directory that is a Git repository, the command prompt will include a name in parentheses.


# Git Clone Recap
The git clone command is used to create an identical copy of an existing repository.
```
$ git clone <path-to-repository-to-clone>
```
This command:

- takes the path to an existing repository
- by default will create a directory with the same name as the repository that's being cloned
- can be given a second argument that will be used as the name of the directory
- will create the new repository inside of the current working directory