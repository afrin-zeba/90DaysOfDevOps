# Day 22 – Introduction to Git: Your First Repository

## Challenge Tasks

### Task 1: Install and Configure Git
1. Verify Git is installed on your machine
git

2. Set up your Git identity — name and email
ubuntu@ip-172-31-46-159:~/devops$ git config --global user.name "afrin-zeba"
ubuntu@ip-172-31-46-159:~/devops$ git config --global user.email "afrinzeba2000@gmail.com"

3. Verify your configuration
ubuntu@ip-172-31-46-159:~/devops-git-practice$ git config --list
user.name=afrin-zeba
user.email=afrinzeba2000@gmail.com
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true

---

### Task 2: Create Your Git Project
1. Create a new folder called `devops-git-practice`
mkdir devops-git-practice
cd devops-git-practice

2. Initialize it as a Git repository
git init 

3. Check the status — read and understand what Git is telling you
git status 

4. Explore the hidden `.git/` directory — look at what's inside
ls -a 
cd .git/
ls -a

---

### Task 3: Create Your Git Commands Reference
1. Create a file called `git-commands.md` inside the repo
touch git-commands.md 

2. Add the Git commands you've used so far, organized by category:
   - **Setup & Config**
   - **Basic Workflow**
   - **Viewing Changes**
3. For each command, write:
   - What it does (1 line)
   - An example of how to use it

╔══════════════════════════════════════════════════════╗
║           🔧 Common Git Commands                     ║
╠══════════════════════════════════════════════════════╣
║  SETUP                                               ║
║  git config --global user.name  "Your Name"          ║
║  git config --global user.email "you@example.com"    ║
╠══════════════════════════════════════════════════════╣
║  BASICS                                              ║
║  git init          Initialize a repo                 ║
║  git clone <url>   Clone a repository                ║
║  git status        Show working tree status          ║
║  git add .         Stage all changes                 ║
║  git add <file>    Stage specific file               ║
║  git commit -m ""  Commit staged changes             ║
║  git push          Push to remote                    ║
║  git pull          Pull from remote                  ║
╠══════════════════════════════════════════════════════╣
║  BRANCHING                                           ║
║  git branch              List branches               ║
║  git branch <name>       Create branch               ║
║  git checkout <name>     Switch branch               ║
║  git checkout -b <name>  Create & switch             ║
║  git merge <branch>      Merge branch                ║
║  git branch -d <name>    Delete branch               ║
╠══════════════════════════════════════════════════════╣
║  HISTORY                                             ║
║  git log               Show commit history           ║
║  git log --oneline     Compact history               ║
║  git diff              Show unstaged changes         ║
║  git diff --staged     Show staged changes           ║
╠══════════════════════════════════════════════════════╣
║  UNDO                                                ║
║  git restore <file>    Discard changes               ║
║  git reset HEAD~1      Undo last commit              ║
║  git stash             Stash changes                 ║
║  git stash pop         Apply stashed changes         ║
╠══════════════════════════════════════════════════════╣
║  REMOTE                                              ║
║  git remote -v              List remotes             ║
║  git remote add origin <url> Add remote              ║
║  git fetch                  Fetch changes            ║
╚══════════════════════════════════════════════════════╝


---

### Task 4: Stage and Commit
1. Stage your file
git add . 

2. Check what's staged
git status 

3. Commit with a meaningful message
git commit -m "git commands checklist"

4. View your commit history
git status 

---

### Task 5: Make More Changes and Build History
1. Edit `git-commands.md` — add more commands as you discover them
2. Check what changed since your last commit
3. Stage and commit again with a different, descriptive message
4. Repeat this process at least **3 times** so you have multiple commits in your history
5. View the full history in a compact format --> git log 

---

### Task 6: Understand the Git Workflow
Answer these questions in your own words (add them to a `day-22-notes.md` file):
1. What is the difference between `git add` and `git commit`?
git add → puts changes into the staging area.
git commit → saves the staged changes into Git history.

2. What does the **staging area** do? Why doesn't Git just commit directly?
The staging area lets you choose exactly what goes into the next commit.
Without a staging area, Git would have to commit all changes together, making commits less organized.

3. What information does `git log` show you?
Commit ID (hash)
Author
Date/time
Commit message


4. What is the `.git/` folder and what happens if you delete it?
.git/ is the hidden folder where Git stores all repository information - commits, branches, tags, configuration, and history.
if we delete the folder the files will remain but the folder will no longer be recognised as a git repo - all commits,branches history etc will be lost 

5. What is the difference between a **working directory**, **staging area**, and **repository**?

Working Directory --> The actual files you're editing.
Staging Area --> A waiting room for changes you want in the next commit
Repository --> The committed history stored in Git.
---

## Ongoing Task

**Keep updating `git-commands.md` every day** as you learn new Git commands in the upcoming days. This will become your personal Git reference. Maintain a clean commit history — one commit per update with a clear message.

---

---

