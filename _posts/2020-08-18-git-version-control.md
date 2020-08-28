--- 
title: Git - Version Control
date: 2020-08-18 14:11 -0400
category: computer science
---

# On Version Control
These are my notes on the version control lecture by [./missing-semester](https://missing.csail.mit.edu/2020/version-control/) and [pro-git](https://git-scm.com/book/en/v2). Also watch the first 24 minutes of this [video](https://www.youtube.com/watch?v=XQs5KcUj-Do) to learn pretty much everything I discuss in this post. 

## What's Version Control?

This refers to a tool that can track changes to source code as well as the encompassing projects folders and files. Let's say you're writing a document in a software such as Word or notepad, you want to be able to keep all the text you've written down so far, thus you "save" the document. At the moment you "save" the document, you are essentially dedicating space on your drive to store the document that your wrote. The idea is simple but effective and essential for writing anything nowadays. Now let's take note that you can save multiple times, which overwrites the last save. What if I want to go back to the previous save? What if I want to see the history of all the saves I've made? Can I revert back to an old save? This is where version control comes in.

## Why do we care?
 
Imagine you're working on a large project and you've just made some change to the code. When you try to run your code, it's not working as intended, it crashes. Something that isn't supposed to be happening, is happening. Well if you had been using a version control system, you could simply revert back to an older version of the project. Also, you can log each change, thus you can actually track and understand the history of changes that happened in that project. That is an invaluable ability, and once you learn this, you will never look back. The bigger picture to visualize is one of collaboration. Not only can you track your changes, but you could have a seperate version of each file, with different changes, thus allowing for individuals to work in parallel on a single project. That's powerful! And that's why companies everywhere use this for everyproject they work on.

# Git conceptually

## Be Better

I really hate the idea that someone would use git without truly understanding what they are doing. Mainy because that's what I used to do. Don't just memorize the main commands and what they do. Strive to understand what exactly is going on. In this manner, you'll have more control over your projects when you implement these systems.

## Why git is powerful

The evolution of version control systems can help aid in understanding this valid question. The progression starts from a simple version database that keeps track of versions, to one where a single server stores many versioned files and people can sort of take it out when they want. But finally comes the current iteration of version control, where both people and servers have copies of the entire version history. 

## The Data Model

First of all, git is one of many, but undoubtedly the most used version control system out there. In git, each file is called a **blob**** and a directory is a **tree**. When we organize a project, often we have folders seperating different parts. Maybe our folder is called "Website Project" and within that are seperate folders for the frontend and backend source code for the project. In this example, our "Website Project" folder is what we can call the root. This in a sense our top-level tree in the data model, or our **snapshot**. What does this all mean? Well note the word snapshot. It is a picture at a certain moment, and in this case, one of our top-level folder containing all the files in our project. So our data model is essentially the collection of trees and blobs at a certain moment, if you will. 

So let's say we have been tracking changes for a while now, and thus accumulated multiple snapshots of our project. How does git keep track of these? 

Suppose you have something like the schema below. For this example, assume that each "O#" represents a single snapshot. Say that the exact contents of each snapshot is different. Maybe the first one had carrots in it, but then the second one had peeled carrots in it, and the final one had cut and peeled carrots in it. In this linear tracking model, each snapshot builds upon the last one. Why are the arrows pointing backwards? Did I write the numbers in reverse order by accident? No! What we are showing here is that snapshots relate in a manner referring to the previouse snapshot. It's like saying "that snapshot came before me" versus, "I came from that snapshot". 

```   
   O1  <--  O2  <-- O3

```
Isn't this pretty much how regular saving works (in word and other applications)? How is this better? Well consider the example below now. We have the same three snapshots, but now there's a new one in another direction. Notice how O3 and O4 are parallel. This implies that they are different snapshots that exist at the same time. The idea is that you can essentially create a fork in the road from O2, resulting in 2 different version of the projects history. Keep this in mind.

```   
   O1  <--  O2  <-- O3
            ^
            |
            ------- O4            
```

### Snapshots

Git refers to snapshots as commits, because you are commiting that version of your project to the system at that time. Each snapshot is more than just the snapshot. There's data describing the data (metadata) such as who made that commit, when did it happen, what was the reason they did it (you can attach messages), errors with commits. 

## How the Structure works

Missing semester whas put down a concise summary of how each data structure works.

```
// a file is a bunch of bytes
type blob = array<byte>

// a directory contains named files and directories
type tree = map<string, tree | blob>

// a commit has parents, metadata, and the top-level tree
type commit = struct {
    parent: array<commit>
    author: string
    message: string
    snapshot: tree
}
```

### Hashing

Git identifies commits through the SHA-1 hash. This concept is moreso used in cybersecurity, but hashing is essentially a way of establishing identity. Basically think of hashes as the resulting of a hashing algorithm, where the algorithm takes in piece of data such as a file, and produces a unique string of a certain length, the hash value. So why go to this effort? Think of millions of projects, each with files containing data. If you have any undertanding of run time, it is very difficult to parse through these files, for whatever reason. If we can assign each piece of data a hash value, and we sort those values in an array, we can immediately access any file. The hash is generated based on the contents of the data, but it can never be reversed, so essentially you have a secure identifier of any file. We don't really care about the security for our purposes, but the ability to immediately find any file and verify that file is very powerful when we think about a code management system. 

### The Data Model Continued

There is an important distinction to be made in how git uses its hashes. Firstly, note that commits are immutable, meaning they are forever unchanging. Think of them as eternal (to a degree). Now since git uses hashes to identify commits, it therefore assigns each commit a hash value. This is a problem for us because we can't read that. So git generates human readable strings for each hash referred to as references. We can use these references, which are mutable unlike the commits themselves, to point to a commit of interest. This is essential to navigation through the git data store

On the topic of the git data store, all items we have discussed are stored as objects, thus why we use references to the hash, and not the actual data itself. Git is very much in a sense navigating metadata. 

So there's some important references to know to understanding how to use git. We have our master reference that refers to the lateset commit in the main branch of a project. If you're looking at a section of the history, whatever version you're currently in is called the **head**. 

Our git data model in sum is a storage of objects containing commits blobs, trees and references, which we call a repository.

# Git for your use

## Git is local

Git is a fast boy. And the reason why git is so fast is because pretty much every operation is occuring at the local level. How it works is that git looks at your local repository, so on your computer, and you can input whatever commands to manipulate files in terms of their version history. 

## Git Workflow

Generally you are only ever adding data to git. The manner in which you do this really works into this idea of 3 states in git. Your repository can be modified, staged or commited. Modified is a sort of limbo state referring to when we are editing our file, but have yet to commit. Next is staged, where we've marked our files for a commit to occur, and then finally the commit state, where the data has actually been stored.

Basically you modify your files in the current directory, our tree, then you stage the changes ready to be part of the next commit and finally you commit them, which gets stored in the git directory. Now the git directory is where metadata and the objects are stored. Now it should be noted that you have 2 main states being tracked (unmodified, modified or staged, or from the last commit) and untracked (files not in snapshot or staging).

## The Git Setup

Let's start of with the command `git config`. You probably won't use this command too much but its got a place in the workflow. When you invoke this command, you can manipulate the look and operation of git itself. There are 3 files that store configuration setings for this command, which can be accessed in these increasingly important (in terms of overriding). `git config --system` applies the configuration values for every system user.  `git config --global` is specific to the user. `git config --local` however does it only to the current repository, which is what it does by default. We can see these settings using `git config --list -show-origin` 

You need to identify yourself so git commits can track this in metadata. Do this using:

```bash 
$ git config --global user.name "Manjot Hunjan"
$ git config --global user.email "myemail@website.com"
```

If you want to remember anything from these commands, remember `git config --list`. This will list all settings for git, and could be useful if you want to manipulate them.

## Help

I don't want to read through documentation and neither do you so let's say you forget how a command works. No worries you got 4 options to quickly get info on how something works. Try them out!

```bash
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
$ git <verb> --h
```

The first 3 produce manualpages, but the last one is a sort of concise version

## The basic commands

So we talked about how a git repository is our collection of objects and references, our objects containing trees, blobs and commits. We have to first generate one. There's 2 ways to go about it, being making a new one (assuming there is no current repository at the working directory) or you can clone one that already exists. 

## Beginning a git project

We initialize using the command `git init`. This generates a folder called .git in your project folder. All we've done is tell github to create a repository here. The other way is to use the `git clone <url>` command. The URL is something you'll have to source from the repository.

So our files can be tracked or untracked. We need to be able to determine the status of our files. We can check that with `git status`. You'll get a few lines pop up like so:

```bash
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
```

## Git Add

First we are told which branch we are on for the commit, and for this instance, assume a branch is a pointer to a commit, and so our master branch like we discussed is a pointer to the most recent commit. So we understand what version of the repository we're working on. The next line tells us if the current branch is consistent with the master branch, because the master branch is always the most recent commit, or our 'default' version of the project. Finally it tells us that we are working in a clean directory, meaning that no tracked files are modified. If we did add a new file, it would be untracked and the status would indicate which those are. So how do we track new files? We use the command `git add <file>`. Now, say you have a bunch of files to commit, or you can't be bothered to remember the name of the file you just updated. No worries! Use this command instead `git add . `. The period acts as a regular expression with the function of meaning the current directory. Essentially what you're telling the computer is `git add all the files in my current directory to the repo`. 

We can also ignore files by creating a .gitignore file and then entering filenames. To make this easier, instead of naming each file, you can use regex. So say we want it to match all files that end with `bob.txt`. We could simply do this `*bob.txt`. What this does is match any filename, of any length, specifically ending with that suffix. There are a whole sort of regex/globbing patterns, which isn't of too much importance but eventually you should learn them. Here's a [cheat-sheet](https://cheatography.com/davechild/cheat-sheets/regular-expressions/) for now. 

## Git Diff

What if we want details on what's been changed? We can use `git diff`, which let's breakdown. For staged changes we can add the tag `--staged`.

``` 
$ git diff

diff --git a/file.txt b/file.txt
index 8ebb991..643e24f 100644
--- a/file.txt
+++ b/file.txt
@@ -1,2 +1 @@ 
- Haha
- Ma
+ Bob's your uncle
```

Let's say you see the above.  First it's telling us there are 2 different files states being version a and version b. You can think of version a as before and version b as the after state. The next line doesn't matter but its those hash values we talked about earlier. The next 2 lines are a legend showing you what symbol match . In this instance, a '-' indicates a change in file 'a' and the '+' indicates a change in file 'b'. You could think of a change as subtracting what used to be there from the old file and adding to the new file. Note that commits are immutable, so we are in no way affecting the previous version of this file, in this example a. All we are doing is generating a new one, being version b. The next line tell you which lines the changes are occuring at, being lines 1 and 2 for file a, and line 1 for file b. Finally we have what actually changed in the last 3 lines. Now the symbols `+` and `-` don't indicate which file it is, rather they indicate what was removed and what was added. In this instant, lines 1,2 had Haha and Ma removed, leaving Bob's your uncle on line 1. 

## Git Commit

Ok so we've staged our files with `git add . `, now we can commit our files, using `git commit`. This will pop up a file for you to enter in a message regarding the commit. Most of the time you only need a one-liner so you can just do `git commit -m 'message'`. 

We can also remove files by using `git rm <filename>`, but note that you would need to delete the file as well so the next time you stage, it doesnt get tracked. What if we just dont want it to be tracked anymore? We can move the file into the .gitignore using `git rm --cached <filename>`. 

## For the Lazy Man

What if you don't want to type `git add .` then `git commit -m <message>`? For you my lazy friend we have this commmand:

```
git commit -am <message>
```
I don't recommend this because it's not really good practice. Imagine

## Git log

The git log command is a highly useful one, in that it allows us to view our commit history. This is what makes git powerful. Oh no your new version of the file isnt working and you can't remember how you broke it? Let's just rollback to an old version of the file. And how do I find an older version, we just do git log and we see something of this format:

```
$ git log

commit <ID>
Author: <Fname Lname email>
Date: <Date + time stamp>

    <git commit message>


commit <ID>
Author: <Fname Lname email>
Date: <Date + time stamp>

    <git commit message>


commit <ID>
Author: <Fname Lname email>
Date: <Date + time stamp>

    <git commit message>

```

Another helpful option is using `git log -p -<number>`, where it returns the log with the updates and the number is just how many entries you want to see in the commit history. 

---
**NOTE**
The -<number> can be used with every log command. If you don't, it opens up a file viewer similar to the output of the `less` command for bash. 
---

It's pretty much a combo of what you see with git log and git diff:

```
$ git log -p -2

commit <ID>
Author: <Fname Lname email>
Date: <Date + time stamp>

    <git commit message>

diff --git a/file.txt b/file.txt
index 8ebb991..643e24f 100644
--- a/file.txt
+++ b/file.txt
@@ -1,2 +1 @@ 
- Haha
- Ma
+ Bob's your uncle

commit <ID>
Author: <Fname Lname email>
Date: <Date + time stamp>

    <git commit message>

diff --git a/file.txt b/file.txt
index 8ebb991..643e24f 100644
--- a/file.txt
+++ b/file.txt
@@ -1,2 +1 @@ 
- Heehee 
+ Santa! 
```

 `git log --stat` tells us a summary of that:
 

```
$ git log --stat

commit <ID>
Author: <Fname Lname email>
Date: <Date + time stamp>

    <git commit message> 

file.txt | <#n of changes> <dashes representing deletions> <pluses representing insertions>
1 file changed, <#n deletions>

commit <ID>
Author: <Fname Lname email>
Date: <Date + time stamp>

    <git commit message>

file.txt | 2 +-
anotherfile.txt | 8 ++++++--
2 files changed, 7 insertions(+), 3 deletions(-)

```

Just ain't pretty enough? We can make this even more concise like this:

```
$git log -pretty=oneline

<id> <message> 
<id> <message> 
<id> <message> 
```

And we can also format our logs such as so:

```
$git log -pretty=format:"%h -%an, %ar : %s"

<id> - <author>, <date> : <message>  
```

Here's a number of the useful specifiers for format:

| Specifier      | Output   |
| :------------- | :----------: |
| %H  | Commit hash   |
| %h  |Abbreviated commit hash|
| %T  |Tree hash|
| %t  |Abbreviated tree hash|
| %P  |Parent hashes|
| %p  |Abbreviated parent hashes|
| %an |Author name|
| %ae |Author email|
| %ad |Author date  |
| %ar |Author date, relative|
| %cn |Committer name|
| %ce |Committer email|
| %cd |Committer date|
| %cr |Committer date, relative|
| %s  |Subject|
 
Out of all these specifiers, the example described above is most useful. If you should however need the exact date, you can change %ar to %ad, to get the date and time stamp.

Remember how we can specify how many commits we want to see with `git log -p`. Well we can even specify if we want to see logs for a specific date, or maybe since 2 weeks ago, or even logs until a date. We can see commits from a specific person using --author <author name>. Perhaps the coolest features is being able to specify commits only matching a specific code. We can do `git log -S <function_name>`. 

## Breakpoint - Let's Recap

So far we know how to make a repository, record changes to that repository and view the histories of commits we've made to the repository. Now what if we want to take back a change?

## Undoing Things

`git commit --amend` will usually be used when you forgot to add a file before a commit. So just add the file with `git add <file>` and then do `git commit --amend`. What's happening is that your replacing your last commit with a new commit. The first commit is removed like it never happened, so you wont see it in your git log. What about files we prepare for a commit (files in the staging area)?

We can use `git reset HEAD <file>`:

```
$ git reset HEAD file.txt

unstaged changes after reset:
M        file.txt
```

Ok so we've unstaged our changes for a commit. Now how do we actually change the file back to the last version. What if my new function doesn't work as efficiently and I want to go back to my old one? We can use:

```
git checkout -- file.txt
```

The pro git documentation warns that these are dangerous commands, and rightfully so. Consider that you will never be able to get the current version back once you head back. OK that's a lie. There are ways to recover data, but I won't talk about those because this is a rather rare occurence (be mindful of what you do!). 

## Remotes

Remote repositories are project versions hosted somewhere other than where you currently are. If you type in `git remote`, you might see the word origin pop up. This simply refers to he repo you originally cloned from.  if we add the option -v, we can see the URL's as well. 

We can have multiple remote repos! Why would you want to do this? Well lets say you want your code saved on github, but you also want it on your server. Just add a new remote for your server via:

``` 
git remote add <name> <url>
```
We can inspect this remote using the command `git remote show <remote name>`, with info such as the URL, all branches available. Don't worry if you dont like your remotes name. Just change it with `git remote rename <oldname> <newname>`. Also you can remove it all together via `git remote remove <name>`. 


One of your most used commands, especially if you interact a lot with github projects will be `git pull`. This lets you fetch and merge data from the remote into your local machine. 

What about the opposite? We can also push our changes after a commit to remotes using `git push <remote name> <branch>`. We'll talk about branches soon! 

## Tagging

Let's say you work in open source and you want to let people know on github that this is version 1.0 of your software. You can do this using git tags! `git tag` will list available tags. To make a new tag, use `git tag -a <tag name> -m "<message>"`. We can tag at certain commits by doing `git tag -a <tag name> <commit ID>`. To push tags to remote servers we need to do it like so: `git push <remote name> <tagname>` or `git push <remoten name> --tags`. Delete tags using the -d flag followed by the tagname, but this is only local. We do this remotely by `git push <remote> --delete <tagname>`. 

We can check different tags using `git checkout <tag name>`, howver the problem with this is that it wont be commited to a branch. We can fix this via `git checkout -b <branch> <tag>` 

## For the lazyman

I dont want to type out all these commands. Too hard to remember!!!! Don't worry my guy. Check out git aliasing! Let's say I want git commit to be called "save". We can do:

```
$git config --global alias.save commit
```
Now instead of `git commit` for comitting our staged changes, we can just type in `git save`. Maybe that's clearer for the mind of a video gamer. IDK. play with it. The more useful thing about aliasing is that we can create custom commands. What if we want a command to see our last commit?

```
git config --global alias.last 'log -1 HEAD'
```
Normally if we wanted to do this, we would have to type in `git log -1`, which is easy but possibly not as easy to remember. Now we just type in `git last`. What if we want to alias our own command? we can do that by repeating the above template, but reference the tool name instead, starting with a ! in the quotations. 

## Branching 

Imagine your project when it start out as a single road. As you add more and more commits, it gets longer and longer. Eventually you come across a problem. I want to be able to try a different route. How do you accomplish this? You need to make an intersection, where you can turn onto another road leading to a different path. Our different route is a so called branch. Before we discuss how it works, why would you care about this in terms of development? Imagine you wanted to make a feature, but you didn't want to put it in your current code, and instead in a copy. All you have to do is make a branch and now there exists two seperate version of the project, or two branches. The master branch, and your new one. 

Recall how the git model works. Given a git repository, when you stage changes with git add, each file gets a SHA-1 hash. These hashes act as identifiers for file versions. Basically it computes the hash, the file gets stored in the git repo and then assigned that hash to the staging area. Then we commit and git makes a hash for the current directory and its subdirectories which is stored as a tree object. A commit object is then created to point to that tree object. In the future, new commits just point to old commit. 

Essentially we have something like this
```
                                        _____<hash2> _____ 
                             -------->  | blob           |
                             |          | contents       |
                             |          |________________|

                      ___<hash1>_____         
____<hash0>____       | tree        |        ____<hash3>_______ 
| commit      |       | blob <hash2>|        | blob           | 
| tree <hash1>|  -->  | blob <hash3>| -----> | contents       | 
|_____________|       | blob <hash4>|        |________________|
                      |_____________|   

                             |
                             |          ______<hash4>_____
                             -------->  | blob           |
                                        | contents       |
                                        |________________|
```
Now what happens when we make a new commit? Notice the parent entry for each commit.

```                   
____<hashA>______       ____<hashB>______       ____<hashC>______
| commit        |       | commit        |       | commit        |        
| tree <hashj>  |  <--  | tree <hashg>  |  <--  | tree <hashk>  |    
| parent        |  <--  | parent <hashA>|  <--  | parent <hashB>|    
|_______________|       |_______________|       |_______________|        
       
       |                     |                     |
       |                     |                     |
       V                     V                     V
_______________       _______________       _______________         
|             |       |             |       |             |
|  Snapshot 1 |       |  Snapshot 2 |       |  Snapshot 3 |
|_____________|       |_____________|       |_____________|
```
To make a branch, we can do `git branch <branch name>`. We can switch between branches using `git checkout <branch name>`. So HEAD refers to the current branch. We can see this using `git log --oneline --decorate`. The diagram below shows what this looks like. Note that we can also see our branches by typing in `git branch`

```
                                    ___________
                                    |         |
                                    |  master |
                                    |_________|
                                    
                                         |
                                         V
___________       ___________       ___________ 
|         |       |         |       |         |
| <hashA> | <---- | <hashB> | <---- | <hashC> |
|_________|       |_________|       |_________|

                                         ^   
                                         |   
                                    ____________
                                    |          |
                                    |  branch1 |
                                    |__________|

                                         ^   
                                         |   
                                    ___________
                                    |         |
                                    |   HEAD  |
                                    |_________|
```

So let's say you or a project collaborator is working on branch 1 and then they stage changes and commit them. What happens to these pointers?

```
                                    ___________
                                    |         |
                                    |  master |
                                    |_________|
                                    
                                         |
                                         V
___________       ___________       ___________       ___________  
|         |       |         |       |         |       |         |
| <hashA> | <---- | <hashB> | <---- | <hashC> | <---- | <hashD> |
|_________|       |_________|       |_________|       |_________|

                                                           ^   
                                                           |   
                                                      ____________
                                                      |          |
                                                      |  branch1 |
                                                      |__________|
  
                                                           ^   
                                                           |   
                                                      ___________
                                                      |         |
                                                      |   HEAD  |
                                                      |_________|
```
AHA! So what we see is that commits only change the current branch. In essence, the master branch is now behind our "branch1". Command-line wise, if we want to see all our branches, we can type in `git log -all`. Now if we `git checkout master`, we'll have something like this going on.


```
                                    ___________
                                    |         |
                                    |   HEAD  |
                                    |_________|

                                         |     
                                         V                                 
                                    ___________            
                                    |         |            
                                    |  master |            
                                    |_________|            

                                         |                 
                                         V                 
___________       ___________       ___________             
|         |       |         |       |         |   
| <hashA> | <---- | <hashB> | <---- | <hashC> | 
|_________|       |_________|       |_________|  <----------
                                                           |
                                                           |
                                                      ___________    
                                                      |         |    
                                                      | <hashD> |
                                                      |_________|
                                                           ^   
                                                           |   
                                                      ____________
                                                      |          |
                                                      |  branch1 |
                                                      |__________|
```

```
                                                     ___________
                                                     |         |
                                                     |   HEAD  |
                                                     |_________|
                    
                                                           |     
                                                           V     
                                                     ___________            
                                                     |         |            
                                                     |  master |            
                                                     |_________|    

                                                           |     
                                                           V     
                                                      ___________
                                                      |         |
                                                      | <hashD1>|
                                                      |_________|
                                                           |
                                                           |
___________       ___________       ___________            | 
|         |       |         |       |         |  <---------- 
| <hashA> | <---- | <hashB> | <---- | <hashC> | 
|_________|       |_________|       |_________|  <----------
                                                           |
                                                           |
                                                      ___________    
                                                      |         |    
                                                      | <hashD> |
                                                      |_________|
                                                           ^   
                                                           |   
                                                      ____________
                                                      |          |
                                                      |  branch1 |
                                                      |__________|
```

Of course we wont see this diagram in git, but we can see something logged into the console with `git log --oneline --decorate --graph --all`. 

## For the lazy man

You don't have to make a new branch then move to it. Just do `git checkout -b <newbranch name>`. 

## Let's Talk Workflow

So let's say your working on a project and you have a couple commits in your history. Then imagine you want to add some new feature, so you make branch to work on it (let's call this feature), but while your working on it, someone tells you something is broken on your project (maybe its a website project and your website stopped working). You can't just leave because you'll have modified files that are not staged or commited. Git won't let you change branches. So you have two options, commit your half-done work on that new feature so you can go work on the other problem OR we can stash the changes.

### Stashing 

`git stash` lets us store our work so we don't lose our work, and we don't have to apply it. Now stashes can be accessed anywhere, so its best to make sure you're on the right branch, in this case the one you just switched out from because your website broke. Then we can go on and work on our other branch to fix the problem, merge it with our master branch and switch back to our new feature branch. So how do we apply our changes? Well we can use `git stash apply` which applies most recent stash, or we can specify one by adding the stashname `<stash@{number}>`. But this doesn't remove the stash. It's always there. If you want to get rid of it, we can use `git stash pop` to use the stash and then immediately get rid of it. Now, the thing with these commands is that all files are put back into the modified format instead of being staged when you apply a stash so say you had something that you were about to commit, and something you were still changing, we can go back to where we left off by adding the --index flag. There's many useful flags but for me, I think `git stash --patch` is the most useful. Basically it asks you wish changes you want to stash.  

What if you want to stash your work, but continue working on the same branch. No worries, just run `git stash branch <branchname>` to work on it. Then we can just merge it when we want. 

What about deleting files you don't want? We could do `git clean -f -d`, but its generally recommended to do `git clean -d n` instead, which shows you what would happen if you ran this command. Note that we are removing only untracked files, so not staged, modified or commited. Or we can just save everythin in a stash and then remove everything `git stash --all`. 

## Back to the flow

Ok so we handled our half-done work and we're back in the master branch to fix that error. Now we need to make a new branch to fix it (ideally you should). Let's call this new branch fixbug. So we switch to fixbug, we make our changes and then we commit them. Now we go back to the master branch and we need to combine master and fixbug, as to incorporate our changes. We do `git merge fixbug` while in the master branch, and voila, those two branches are now combined. Now we should note that what actually happened is that the master branch is now pointing forward, which is why you see the output "fast-forward".

However, the fixbug branch still exists, and we don't need it anymore, so we delete it via `git branch -d fixbug`. Ok so what about our feature branch? Well we go checkout to that branch, do our work and once we're done, commit the changes. Ok now if we think about our git comit history, it actually looks like this right now:

```
                      Master
                        |
                        V
C0 <-- C1 <--- C2 <--- C4
                ^
                |----- C3 <--- C5
                                ^
                                |
                              Feature 
```
We can solve this using `git merge feature` on the master branch. Now the master branch will pointer will be pointed to a new commit, lets say C6,  which points back at C5 and C4. Then of course we delete the feature branch when we're done with it. In practice, this doesn't always work because of merge conflicts.  For example, if someone on the master branch changed line 1 of file.txt and someone on feature did the same thing, when we merge, there would be an issue. If you check `git status`, it will show you which paths are unmerged. Simply open the file and git will automatically insert the two different lines into your code. Simply decide how you want it to be, then `git add . | git commit -m 'merged branches'`.

There is a way to have a cleaner history, meaning not having these weird paths connecting back to each other. If we use `git rebase <branch to rebase onto>` while in the branch we are "merging", it just generates a new commit pointing to the master commit, with changes based on the new branch. then we just use git merge on the master branch to fast forward it. You don't want to do rebasing on commits outside the repo and with people working on that version. Your essentially erasing history with rebases.

`git reset --hard 'commit name'` lets us go back to a previous version of the commits.