--- 
title: Git - Version Control
date: 2020-08-18 14:11 -0400
category: computer science
---

# On Version Control
These are my notes on the version control lecture by [./missing-semester](https://missing.csail.mit.edu/2020/version-control/) and [pro-git](https://git-scm.com/book/en/v2).

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
git config --global user.name "Manjot Hunjan"
git config --global user.email "myemail@website.com"
```

If you want to remember anything from these commands, remember `git config --list`. This will list all settings for git, and could be useful if you want to manipulate them.

## Help

I don't want to read through documentation and neither do you so let's say you forget how a command works. No worries you got 4 options to quickly get info on how something works. Try them out!

```bash
git help <verb>
git <verb> --help
man git-<verb>
git <verb> --h
```

The first 3 produce manualpages, but the last one is a sort of concise version

## The basic commands

So we talked about how a git repository is our collection of objects and references, our objects containing trees, blobs and commits. We have to first generate one. There's 2 ways to go about it, being making a new one (assuming there is no current repository at the working directory) or you can clone one that already exists. 

We initialize using the command `git init`. This generates a folder called .git in your project folder. All we've done is tell github to create a repository here. The other way is to use the `git clone <url>` command. The URL is something you'll have to source from the repository.

So our files can be tracked or untracked. We need to be able to determine the status of our files. We can check that with `git status`. You'll get a few lines pop up like so:

```bash
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
```
First we are told which branch we are on for the commit, and for this instance, assume a branch is a pointer to a commit, and so our master branch like we discussed is a pointer to the most recent commit. So we understand what version of the repository we're working on. The next line tells us if the current branch is consistent with the master branch, because the master branch is always the most recent commit, or our 'default' version of the project. Finally it tells us that we are working in a clean directory, meaning that no tracked files are modified. If we did add a new file, it would be untracked and the status would indicate which those are. So how do we track new files? We use the command `git add <file>`. Now, say you have a bunch of files to commit, or you can't be bothered to remember the name of the file you just updated. No worries! Use this command instead `git add . `. The period acts as a regular expression with the function of meaning the current directory. Essentially what you're telling the computer is `git add all the files in my current directory to the repo`. 

We can also ignore files by creating a .gitignore file and then entering filenames. To make this easier, instead of naming each file, you can use regex. So say we want it to match all files that end with `bob.txt`. We could simply do this `*bob.txt`. What this does is match any filename, of any length, specifically ending with that suffix. There are a whole sort of regex/globbing patterns, which isn't of too much importance but eventually you should learn them. Here's a [cheat-sheet](https://cheatography.com/davechild/cheat-sheets/regular-expressions/) for now. 

What if we want details on what's been changed? We can use `git diff`, which let's breakdown. For staged changes we can add the tag `--staged`.

``` 
 git diff
diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
index 8ebb991..643e24f 100644
--- a/CONTRIBUTING.md
+++ b/CONTRIBUTING.md
@@ -1,2 +1 @@ 
- Haha
- Ma
+ Bob's your uncle
```

Let's say you see the above.  First it's telling us there are 2 different files states being version a and version b. You can think of version a as before and version b as the after state. The next line doesn't matter but its those hash values we talked about earlier. The next 2 lines are a legend showing you what symbol match . In this instance, a '-' indicates a change in file 'a' and the '+' indicates a change in file 'b'. You could think of a change as subtracting what used to be there from the old file and adding to the new file. Note that commits are immutable, so we are in no way affecting the previous version of this file, in this example a. All we are doing is generating a new one, being version b. The next line tell you which lines the changes are occuring at, being lines 1 and 2 for file a, and line 1 for file b. Finally we have what actually changed in the last 3 lines. Now the symbols `+` and `-` don't indicate which file it is, rather they indicate what was removed and what was added. In this instant, lines 1,2 had Haha and Ma removed, leaving Bob's your uncle on line 1. 

Ok so we've staged our files with `git add . `, now we can commit our files, using `git commit`. This will pop up a file for you to enter in a message regarding the commit. Most of the time you only need a one-liner so you can just do `git commit -m 'message'`. 

We can also remove files by using `git rm <filename>`, but note that you would need to delete the file as well so the next time you stage, it doesnt get tracked. What if we just dont want it to be tracked anymore? We can move the file into the .gitignore using `git rm --cached <filename>`. 

## Git log

The git log command is a highly useful one that 