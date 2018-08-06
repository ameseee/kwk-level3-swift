
footer: KWK Swift/iOS: Git & GitHub
slidenumbers: true

# Git & GitHub

^ Instructor: Whether you're a solo developer or working on a team, you should be using some kind of source control for your projects. Git is actually built right into Xcode, so it makes it really easy to use. However, today we're going to expand outside of Xcode and learn some of the basics of git that will translate to any project, whether we are using Xcode, Atom, VSCode, or any other IDE (integrated development environment) for project development.

---

# Learning Goals

* Understand what source control is and why it is useful
* Understand the differences in Git and GitHub and how they work together
* Be able to create a new git repository
* Know how to add and commit files and push them up to GitHub

---

# Vocab

* Git 
* GitHub
* Git commands
  - init, status, add, commit, log, push, pull
 
---

# What is Git?

* Git is a version control system 
* We interact with Git from the command line (terminal)
* We use Git to commit changes to our project
* These commits serve as snapshots in time (or versions) of our project that we can save in a local repository on our computer


^ VCS is a tool that keeps track of the differences in code bases at different points in time. A project that uses Git has a complete history of the code changes throughout its existence, which can be powerful if you want to look at the origin of a bug, or if you want to revert to an old version of a project. Git is used throughout the software development community, not only for web development - it gives developers a robust way to collaborate, share, and maintain code bases. 

---

# What is GitHub?

* Despite their similar names, Git is NOT the same as GitHub
* GitHub is an online Git project repository hosting service
* GitHub allows teams to work together on the same codebase
* [GitHub Video](https://www.youtube.com/watch?v=w3jLJU7DT5E)

^ Git project repository hosting service... which means it holds the directories that contain all the files and folders that make up our projects. Everyone on a team can pull down a local version of the repo on GitHub, and then, as work is done, the code is committed and pushed from the developer’s local repo and added to the repo on GitHub.

---

# Advantages of Using Source Control

* Helps you more easily revert to older versions of your code
* Allows you to add features without risk to your working app
* Allows you to see how your code has changed over time
* If you are working on a team, new contributors can jump in and see the evolution of the project

---

# Getting started with Git

* The first thing we need to do is initialize a new git repository (on our local machine) 
* From the command line in your terminal, navigate to your project folder and run `git init`
* You'll see something like this in return... `Initialized empty Git repository in /Users/username/whatever/directory/.git/`

^ *git init*: creates a new Git repository 

---

# Connecting our project to GitHub

* Go to your GitHub account and click the + button in the top right corner of the page
* Select `New repository` and add a name for your repository, then click `Create repository`
* Copy and paste the line in the second section that looks like this... `git remote add origin git@github.com:christielynam/nameofrepo.git`


^ Only 1 person that is contributing to the project needs to do this.  Other contributors will just be cloning down your repository.

---

# Where our code lives (within git)

* Working Area 
  - Where **unstaged files** live
* Staging Area 
  - Where files that are going to be a part of the next commit live
* The Repository
  - The files git knows about
  - Contains all of your commits

---

# Git Workflow

![inline](./slide_images/git-add-commit.png)

---

# Making your Initial Commit

* `git add .`
* `git commit -m "Initial commit"`
* `git push origin master`

^ We'll break each of these down in the next few slides

---

# Adding a file to the staging area (git add)

* At any time, we can run `git status` to get some info about our project
  - branch info, commit info, untracked files, changes to be committed
* To add a specific file to the staging area, run `git add filename`
* If you want to add all untracked files to the staging area, you can run `git add .`

^ *git status*: inspects the contents of the working directory and staging area
*git add*: adds files from the working directory to the staging area

---

# Committing files (git commit)

* Committing a file will "save" our changes to our git history
* Think of this as a snapshot of our project
* Commit a file using the command: `git commit -m "Initial commit"`

^ *git commit*: permanently stores file changes from the staging area in the repository
The `-m` part of the command says we want to add a message to our commit. Always add a commit message. The message is information that you (or other developers) can use at a later time when you want to look back and see what changes were contained in a commit.

---

# Basic rules of a decent commit message

* Capitalize the first letter
* Do not end the message with a period
* Use the imperative mood
* Explain what and why vs. how

---

# Check our status

* This is a good time to run `git status` again
  - make sure we have added and committed all the files we have been working on
* You can also run `git log` to see a list of all your commits

^ *git log*: shows a list of all previous commits (has the hash/sha... you need this to revert back to a previous commit)

---

# Pushing to GitHub

* Up until this point, all our changes have taken place and been tracked on our local machine
* When we are satisfied with our updated code, we can run the command `git push origin master`
* This will add our code to our remote repository on GitHub - now it's in the cloud and can be accessed by others

---

# Pull remote master into our local version

* Other contributors to our project need to be able to access our updated code and incorporate it into their local versions of the project on their machine
* To get the changes from the master branch on GitHub to our local master branch, we need to "pull" down those changes
* To do this, run the command `git pull origin master`

^ Our changes are now completely synced. The master branch locally is the same as the master branch on GitHub. 

---

# Model Git Workflow

* Christie and Amy Roadshow