# Merge Conflict Output Explained
The output that shows in the Terminal is:
```
$ git merge heading-update 
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```
Notice that right after the ```git merge heading-update``` command, it tries merging the file that was changed on both branches (index.html), but that there was a conflict. Also, notice that it tells you what happened - "Automatic merge failed; fix conflicts and then commit the result".


Remember our good friend ```git status```? Well he'll come in really handy when working with merge conflicts.

# Merge Conflict Indicators Explanation
The editor has the following merge conflict indicators:

- ```<<<<<<< HEAD``` everything below this line (until the next indicator) shows you what's on the current branch
- ```||||||| merged common ancestors``` everything below this line (until the next indicator) shows you what the original lines were
- ```=======``` is the end of the original lines, everything that follows (until the next indicator) is what's on the branch that's being merged in
- ```>>>>>>> heading-update``` is the ending indicator of what's on the branch that's being merged in (in this case, the ```heading-update``` branch)

# Resolving A Merge Conflict
Git is using the merge conflict indicators to show you what lines caused the merge conflict on the two different branches as well as what the original line used to have. So to resolve a merge conflict, you need to:

- choose which line(s) to keep
- remove all lines with indicators

# Commit Merge Conflict
Once you've removed all lines with merge conflict indicators and have selected what heading you want to use, just save the file, add it to the staging index, and commit it! Just like with a regular merge, this will pop open your code editor for you to supply a commit message. Just like before, it's common to use the provided merge commit message, so after the editor opens, just close it to use the provided commit message.

And that's it! Merge conflicts really aren't all that challenging once you understand what the merge conflict indicators are showing you.

# Merge Conflict Recap
A merge conflict happens when the same line or lines have been changed on different branches that are being merged. Git will pause mid-merge telling you that there is a conflict and will tell you in what file or files the conflict occurred. To resolve the conflict in a file:

- locate and remove all lines with merge conflict indicators
- determine what to keep
- save the file(s)
- stage the file(s)
- make a commit
Be careful that a file might have merge conflicts in multiple parts of the file, so make sure you check the entire file for merge conflict indicators - a quick search for <<< should help you locate all of them.