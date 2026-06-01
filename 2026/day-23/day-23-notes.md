## Challenge Tasks

### Task 1: Understanding Branches
Answer these in your `day-23-notes.md`:
1. What is a branch in Git?
branch is a way in git to branch out from main such that we can make changes to files without directly modifying the main 

2. Why do we use branches instead of committing everything to `main`?
branches allow us to 
- work without disrupting main 
- facilitates collaboration of multiple people working on the same file 
- review changes properly before merging to main 

3. What is `HEAD` in Git?
HEAD is a pointer that tells Git where you currently are --> Usually it points to the latest commit of the branch you're working on.

4. What happens to your files when you switch branches?
if the files havent been created in the other branch they would not be visible - git switches the working durectory to whatever is present based on the branch 
---

### Task 2: Branching Commands — Hands-On
In your `devops-git-practice` repo, perform the following:
1. List all branches in your repo
git branch 

2. Create a new branch called `feature-1`
git checkout -b feature-1

3. Switch to `feature-1`
git switch feature-1

4. Create a new branch and switch to it in a single command — call it `feature-2`
git checkout -b feature-2

5. Try using `git switch` to move between branches — how is it different from `git checkout`?
git checkout allows you to recover files but got switch doesnt - in all other ways git switch is the ew version of checkout 
git checkout -b vs got switch ic --> to create new branch 

6. Make a commit on `feature-1` that does **not** exist on `main`
git switch feature-1
git commit -m "first commit"

7. Switch back to `main` — verify that the commit from `feature-1` is not there
git switch master

8. Delete a branch you no longer need
You cannot delete the branch you're currently on.
For example, if you're on feature-login:

git switch main
git branch -d feature-login
You must switch to another branch first.

9. Add all branching commands to your `git-commands.md`

---

### Task 3: Push to GitHub
1. Create a **new repository** on GitHub (do NOT initialize it with a README)
2. Connect your local `devops-git-practice` repo to the GitHub remote
3. Push your `main` branch to GitHub
4. Push `feature-1` branch to GitHub
5. Verify both branches are visible on GitHub
6. Answer in your notes: What is the difference between `origin` and `upstream`?
origin = your copy of the repository (your fork)
upstream = the original repository you forked from

---

### Task 4: Pull from GitHub
1. Make a change to a file **directly on GitHub** (use the GitHub editor)
2. Pull that change to your local repo
git pull origin master
3. Answer in your notes: What is the difference between `git fetch` and `git pull`?
git fetch - Downloads the latest changes from the remote repository but does not modify your current branch.
"Show me what's new on the remote, but don't apply it yet."

git pull

Downloads the latest changes and immediately merges them into your current branch.
"Get the latest changes and update my branch right now."
---

### Task 5: Clone vs Fork
1. **Clone** any public repository from GitHub to your local machine
git clone "link"
2. **Fork** the same repository on GitHub, then clone your fork
click on fork 
then git clone "fork-url"
3. Answer in your notes:
   - What is the difference between clone and fork?
   - When would you clone vs fork?
   clone is creating a copy of the repo to your local machine, whereas fork creates a copy of a repository on GitHub under your account
    Use clone when you want a local copy of a repository to work on. eg:- Working on your team's repository
    
    Fork
    Use fork when you want your own GitHub copy of someone else's repository.
    Contributing to an open-source project
    Experimenting without affecting the original repository

   - After forking, how do you keep your fork in sync with the original repo?
   Add the original repository as upstream
   git remote add upstream https://github.com/original-owner/repo.git
   
   Fetch latest changes from upstream
   git fetch upstream

   Update your local main branch
   git switch main
   git merge upstream/main
   git push origin main
    ---

## Hints
- When you create a branch, it starts from the commit you're currently on
- `git switch` is the modern alternative to `git checkout` for switching branches
- To push a new branch: `git push -u origin <branch-name>`
- A fork is a GitHub concept, not a Git concept

