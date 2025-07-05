# Getting Started with Git

Git is a distributed version control system that helps track changes to your code and collaborate with others. This tutorial will guide you through the basics of using Git.

## Table of Contents
1. [Installation](#installation)
2. [Configuration](#configuration)
3. [Basic Git Commands](#basic-git-commands)
4. [Working with Remote Repositories](#working-with-remote-repositories)
5. [Branching and Merging](#branching-and-merging)
6. [Best Practices](#best-practices)

## Installation

### Windows
1. Download Git from [git-scm.com](https://git-scm.com/download/win)
2. Run the installer and follow the instructions
3. Verify installation by opening Command Prompt or PowerShell and typing:
   ```
   git --version
   ```

### macOS
1. Install via Homebrew:
   ```
   brew install git
   ```
2. Or download from [git-scm.com](https://git-scm.com/download/mac)
3. Verify installation:
   ```
   git --version
   ```

### Linux
1. For Debian/Ubuntu:
   ```
   sudo apt-get update
   sudo apt-get install git
   ```
2. For Fedora:
   ```
   sudo dnf install git
   ```
3. Verify installation:
   ```
   git --version
   ```

## Configuration

After installing Git, set up your identity:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

Optional: Set your default editor:
```bash
git config --global core.editor "code --wait"  # For Visual Studio Code
```

## Basic Git Commands

### Initializing a Repository
```bash
git init
```

### Checking Status
```bash
git status
```

### Adding Files to Staging Area
```bash
git add filename.txt    # Add specific file
git add .               # Add all files
```

### Committing Changes
```bash
git commit -m "Your commit message here"
```

### Viewing Commit History
```bash
git log
git log --oneline       # Compact view
```

## Working with Remote Repositories

### Clone a Repository
```bash
git clone https://github.com/username/repository.git
```

### Add a Remote
```bash
git remote add origin https://github.com/username/repository.git
```

### Push Changes
```bash
git push origin main    # Push to main branch
```

### Pull Changes
```bash
git pull origin main    # Pull from main branch
```

## Branching and Merging

### Create a New Branch
```bash
git branch new-feature
git checkout new-feature
# OR one command:
git checkout -b new-feature
```

### Switch Between Branches
```bash
git checkout main
```

### Merge Branches
```bash
git checkout main
git merge new-feature
```

### Delete a Branch
```bash
git branch -d new-feature    # Safe delete (if merged)
git branch -D new-feature    # Force delete
```

## Best Practices

1. **Commit Often**: Make small, focused commits that address a single issue or feature
2. **Write Clear Commit Messages**: Use present tense and be descriptive
3. **Pull Before Push**: Always pull the latest changes before pushing to avoid conflicts
4. **Use Branches**: Create separate branches for new features or bug fixes
5. **Review Changes Before Committing**: Use `git diff` to review changes before staging

## Common Issues and Solutions

### Undo Last Commit (Keep Changes)
```bash
git reset --soft HEAD~1
```

### Discard All Local Changes
```bash
git reset --hard HEAD
```

### Resolve Merge Conflicts
When conflicts occur:
1. Open the conflicted files
2. Look for conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
3. Edit the files to resolve conflicts
4. Add the resolved files and commit:
   ```bash
   git add .
   git commit -m "Resolve merge conflicts"
   ```

## Conclusion

This tutorial covered the basics of using Git for version control. As you get more comfortable with these commands, you'll find Git to be an invaluable tool for managing your code and collaborating with others.

For more advanced Git features and detailed documentation, visit the [official Git website](https://git-scm.com/doc).
