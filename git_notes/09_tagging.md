# Tagging
The idea behind git tag is to provide a way to have a commit stand out from the other commits.
We do so by tagging a commit. A tag stays lock in with that commit

# Git Tag Command
Pay attention to what's shown (just the SHA and the commit message)

The command we'll be using to interact with the repository's tags is the ```git tag``` command:
```
$ git tag -a v1.0
```
This will open your code editor and wait for you supply a message for the tag. How about the message "Ready for content"?

> CAREFUL: In the command above (```git tag -a v1.0```) the ```-a``` flag is used. This flag tells Git to create an annotated flag. If you don't provide the tag (i.e. ```git tag v1.0```) then it'll create what's called a lightweight tag.

> Annotated tags are recommended because they include a lot of extra information such as:

> - the person who made the tag
> - the date the tag was made
> - a message for the tag
> Because of this, you should always use annotated tags.

# Verify Tag
After saving and quitting the editor, nothing is displayed on the command line. So how do we know that a tag was actually added to the project? If you type out just ```git tag```, it will display all tags that are in the repository.
![git tag](git_img/ud123-l5-git.tag.png)
The Terminal application showing the output of the git tag command. The tag v1.0 is listed.
So we've verified that it's in the repository, but let's actually see where it is inside the repository. To do that, we'll go back to our good old friend, ```git log```!

# Git Log's --decorate Flag
As you've learned, ```git log``` is a pretty powerful tool for letting us check out a repository's commits. We've already looked at a couple of its flags, but it's time to add a new one to our toolbelt. The ```--decorate``` flag will show us some details that are hidden from the default view.
Try running git log ```--decorate``` now!

> **Note that in git 2.13, git log has --decorate enabled by default**

The tag information is at the very end of the first line:
```
commit 6fa5f34790808d9f4dccd0fa8fdbc40760102d6e (HEAD -> master, tag: v1.0)
```
See how it say ```tag: v1.0```? That's the tag! Remember that tags are associated with a specific commit. This is why the tag is on the same line as the commit's SHA.

# Deleting A Tag
What if you accidentally misspelled something in the tag's message, or mistyped the actual tag name (```v0.1``` instead of ```v1.0```). How could you fix this? The easiest way is just to delete the tag and make a new one.

A Git tag can be deleted with the ```-d``` flag (for delete!) and the name of the tag:
```
$ git tag -d v1.0
```
![git tag delete](git_img/ud123-l5-git-tag-delete.png)

# Adding A Tag To A Past Commit
Running ```git tag -a v1.0``` will tag the most recent commit. But what if you wanted to tag a commit that occurred farther back in the repo's history?

All you have to do is provide the SHA of the commit you want to tag!
```
$ git tag -a v1.0 a87984
```
(after popping open a code editor to let you supply the tag's message) this command will tag the commit with the SHA ```a87084``` with the tag ```v1.0```. Using this technique, you can tag any commit in the entire git repository! Pretty neat, right?...and it's just a simple addition to add the SHA of a commit to the Git tagging command you already know.

# Git Tag Recap
To recap, the ```git tag``` command is used to add a marker on a specific commit. The tag does not move around as new commits are added.
```
$ git tag -a beta
```
This command will:

- add a tag to the most recent commit
- add a tag to a specific commit if a SHA is passed
