# **GitHub Notes**

## GitHub Workflow: Add, Commit, and Push

### 1. Add

The 'add' command stages changes for commit:

```bash
git add [file]  # Stage a specific file
git add .       # Stage all changes in the current directory
git add -A      # Stage all changes in the repository

git status      # To check which files are added to staging area.
```

### 2. Commit

The 'commit' command saves the staged changes to local repo:

```bash
git commit -m "Your commit message here"

git log    # To get the log the committed files to local repo.
```

Best practices for commit messages:

- Keep it concise and descriptive
- Use present tense (e.g., "Add feature" not "Added feature")
- Limit the first line to 50 characters

### 3. Push

The 'push' command uploads local commits to a remote repository:

```bash
git push -u origin [branch-name]
```

Example: Pushing to the main branch

```bash
git push -u origin main
```

### Complete Workflow Example

```bash
# Make changes to your files

# Stage changes
git add .

# Commit changes
git commit -m "Your message."

# Push changes to remote repository
git push -u origin main
```

Remember to pull changes from the remote repository before pushing to avoid conflicts:

```bash
git pull origin main
```

### Creating and Pushing to a New Remote Repository

To create a new remote repository and push your local files to it, follow these steps:

1. Create a new repository on GitHub (or your preferred Git hosting service) without initializing it with a README, license, or .gitignore files.
2. Copy the URL of your new repository.
3. Open your terminal and navigate to your local project directory.
4. Initialize the local directory as a Git repository (if not already done):

```bash
git init
```

1. Add the files in your new local repository:

```bash
git add .
```

1. Commit the files you've staged in your local repository:

```bash
git commit -m "Initial commit"
```

1. Add the URL for the remote repository where your local repository will be pushed:

```bash
git remote add origin [remote repository URL]
```

1. Push the changes in your local repository to GitHub:

```bash
git push -u origin main
```

Note: If your default branch is named differently (e.g., 'master' instead of 'main'), replace 'main' with your branch name in the last command.

After these steps, your local repository will be pushed to the new remote repository on GitHub.

# GitHub Branching and Merging

### 1. Creating a Branch

Create a new branch to work on a feature or fix:

```bash
git branch [branch-name]
git checkout -b [branch-name]  # Create and switch to new branch
```

### 2. Switching Branches

Move between branches:

```bash
git branch    #To see present working branch and total branches

git checkout [branch-name] #To move to the particular branch
```

### 3. Working on a Branch

Make changes, add, and commit as usual:

```bash
git add .
git commit -m "Your commit message"
```

### 4. Pushing a Branch

Push your branch to the remote repository:

```bash
git push -u origin [branch-name]
```

### 5. Merging to Main Branch

First, switch to the main branch:

```bash
git checkout main
```

Then, merge your branch into main:

```bash
git merge [branch-name]
```

### 6. Resolving Merge Conflicts

If conflicts occur, resolve them manually in the conflicting files. Then:

```bash
git add .
git commit -m "Resolve merge conflicts"
```

### 7. Pushing Merged Changes

After merging, push the changes to the remote main branch:

```bash
git push origin main
```

### 8. Deleting a Branch

Once merged, you can delete the branch locally and remotely:

```bash
git branch -d [branch-name]  # Delete local branch
git push origin --delete [branch-name]  # Delete remote branch
```

Remember to always pull the latest changes from the main branch before creating a new branch or merging to avoid conflicts.
