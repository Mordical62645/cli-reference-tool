# Git Basic Commands and Workflows

## Getting Started
- `git --version` - Check Git version
- `git config --global user.name "Your Name"` - Set global username
- `git config --global user.email "your.email@example.com"` - Set global email
- `git config --list` - View all Git configurations
- `git init` - Initialize new Git repository
- `git clone url` - Clone remote repository
- `git clone url directory_name` - Clone to specific directory

## Repository Status
- `git status` - Check repository status
- `git status -s` - Short status format
- `git log` - View commit history
- `git log --oneline` - Compact commit history
- `git log --graph --oneline --all` - Visual commit history
- `git log -p filename` - Show changes for specific file
- `git show commit_hash` - Show details of specific commit

## File Operations
- `git add filename` - Stage specific file
- `git add .` - Stage all changes
- `git add *.txt` - Stage all .txt files
- `git reset filename` - Unstage file
- `git checkout -- filename` - Discard changes in working directory
- `git rm filename` - Remove file from Git and working directory
- `git mv oldname newname` - Rename file

## Committing Changes
- `git commit -m "commit message"` - Commit staged changes
- `git commit -am "commit message"` - Stage and commit tracked files
- `git commit --amend` - Amend last commit
- `git commit --amend -m "new message"` - Amend last commit with new message

## Branching
- `git branch` - List all branches
- `git branch -a` - List all branches (local and remote)
- `git branch branch_name` - Create new branch
- `git checkout branch_name` - Switch to branch
- `git checkout -b branch_name` - Create and switch to new branch
- `git branch -m old_name new_name` - Rename current branch
- `git branch -m old_name new_name` - Rename specific branch
- `git branch -d branch_name` - Delete branch (if merged)
- `git branch -D branch_name` - Force delete branch
- `git merge branch_name` - Merge branch into current branch
- `git merge --abort` - Abort merge in progress

## Remote Operations
- `git remote -v` - List remote repositories
- `git remote add origin url` - Add remote repository
- `git remote remove origin` - Remove remote repository
- `git fetch origin` - Download changes from remote
- `git pull origin branch_name` - Pull and merge changes
- `git pull --rebase origin branch_name` - Pull with rebase
- `git push origin branch_name` - Push branch to remote
- `git push -u origin branch_name` - Push and set upstream
- `git push --force origin branch_name` - Force push (use with caution)

## Basic GitHub Workflow
### Initial Setup
```bash
# Clone repository
git clone https://github.com/username/repository.git
cd repository

# Or initialize and connect to remote
git init
git remote add origin https://github.com/username/repository.git
```

### Daily Workflow
```bash
# 1. Get latest changes
git pull origin main

# 2. Make your changes
# ... edit files ...

# 3. Stage and commit changes
git add .
git commit -m "feat: add new feature"

# 4. Push to remote
git push origin main
```

### Working with Branches
```bash
# 1. Create and switch to feature branch
git checkout -b feature/new-feature

# 2. Make changes and commit
git add .
git commit -m "feat: implement new feature"

# 3. Push new branch to remote
git push -u origin feature/new-feature

# 4. After review, merge to main
git checkout main
git pull origin main
git merge feature/new-feature
git push origin main

# 5. Clean up
git branch -d feature/new-feature
git push origin --delete feature/new-feature
```

## Stashing
- `git stash` - Stash changes temporarily
- `git stash save "message"` - Stash with message
- `git stash list` - List all stashes
- `git stash pop` - Apply and remove latest stash
- `git stash apply stash@{n}` - Apply specific stash
- `git stash drop stash@{n}` - Remove specific stash
- `git stash clear` - Remove all stashes

## Tagging
- `git tag` - List all tags
- `git tag tag_name` - Create lightweight tag
- `git tag -a tag_name -m "message"` - Create annotated tag
- `git push origin tag_name` - Push specific tag
- `git push origin --tags` - Push all tags
- `git tag -d tag_name` - Delete local tag

## Diff and Comparison
- `git diff` - Show unstaged changes
- `git diff --staged` - Show staged changes
- `git diff HEAD~1` - Compare with previous commit
- `git diff branch1..branch2` - Compare two branches
- `git diff commit1..commit2` - Compare two commits

## Undoing Changes
- `git reset --soft HEAD~1` - Undo last commit, keep changes staged
- `git reset --mixed HEAD~1` - Undo last commit, unstage changes
- `git reset --hard HEAD~1` - Undo last commit, discard changes
- `git revert commit_hash` - Create new commit that undoes changes
- `git checkout commit_hash` - Checkout specific commit (detached HEAD)
- `git checkout HEAD -- filename` - Restore file to last commit

## Information and Help
- `git help command` - Get help for specific command
- `git blame filename` - Show who changed what and when
- `git show-branch` - Show branch relationships
- `git reflog` - Show reference log
- `git shortlog` - Summarize git log output

## Basic Scripts

### 1. Repository Setup Script
```bash
#!/bin/bash
# Initialize new Git repository with basic setup
REPO_NAME="$1"
if [ -z "$REPO_NAME" ]; then
    echo "Usage: $0 <repository_name>"
    exit 1
fi

mkdir "$REPO_NAME"
cd "$REPO_NAME"
git init
echo "# $REPO_NAME" > README.md
git add README.md
git commit -m "Initial commit: Add README"
echo "Repository '$REPO_NAME' initialized successfully!"
```

### 2. Backup Script
```bash
#!/bin/bash
# Backup Git repository
REPO_PATH="$1"
BACKUP_DIR="$2"
DATE=$(date +%Y%m%d_%H%M%S)

if [ -z "$REPO_PATH" ] || [ -z "$BACKUP_DIR" ]; then
    echo "Usage: $0 <repo_path> <backup_dir>"
    exit 1
fi

cd "$REPO_PATH"
git bundle create "$BACKUP_DIR/repo_backup_$DATE.bundle" --all
echo "Backup created: repo_backup_$DATE.bundle"
```

### 3. Clean Repository Script
```bash
#!/bin/bash
# Clean up Git repository
echo "Cleaning Git repository..."

# Remove untracked files and directories
git clean -fd

# Remove files from .gitignore
git clean -X

# Prune remote-tracking branches
git remote prune origin

# Garbage collect
git gc --prune=now

echo "Repository cleaned successfully!"
```

### 4. Branch Management Script
```bash
#!/bin/bash
# Manage Git branches
case "$1" in
    "list")
        echo "Local branches:"
        git branch
        echo -e "\nRemote branches:"
        git branch -r
        ;;
    "cleanup")
        echo "Deleting merged branches..."
        git branch --merged | grep -v "\*" | xargs -n 1 git branch -d
        echo "Cleanup completed!"
        ;;
    "sync")
        echo "Syncing with remote..."
        git fetch origin
        git pull origin $(git branch --show-current)
        ;;
    *)
        echo "Usage: $0 {list|cleanup|sync}"
        exit 1
        ;;
esac
```

### 5. Commit Template Script
```bash
#!/bin/bash
# Create commit with template
TEMPLATE="
# Type: feat|fix|docs|style|refactor|test|chore
# Scope: (optional) component name
# Subject: short description

# Body: (optional) detailed description
# Footer: (optional) breaking changes, issue references

# Example:
# feat: add user authentication
# 
# Implemented JWT-based authentication system
# with refresh token support
# 
# Closes #123
"

echo "$TEMPLATE" > .gitmessage
git config commit.template .gitmessage
echo "Commit template configured!"
```

## Common Workflows

### Feature Branch Workflow
```bash
# Start new feature
git checkout -b feature/new-feature
# Make changes
git add .
git commit -m "feat: implement new feature"
git push -u origin feature/new-feature
# Create pull request on GitHub/GitLab
# After review, merge and cleanup
git checkout main
git pull origin main
git branch -d feature/new-feature
```

### Hotfix Workflow
```bash
# Create hotfix branch from main
git checkout -b hotfix/critical-fix
# Make urgent changes
git add .
git commit -m "fix: critical security issue"
git push origin hotfix/critical-fix
# Merge to main and develop
git checkout main
git merge hotfix/critical-fix
git push origin main
git checkout develop
git merge hotfix/critical-fix
git push origin develop
git branch -d hotfix/critical-fix
```

### Rebase Workflow
```bash
# Keep feature branch up to date
git checkout feature/branch
git fetch origin
git rebase origin/main
# Resolve conflicts if any
git push --force-with-lease origin feature/branch
```

## Git Configuration Examples

### Global Configuration
```bash
# Set default editor
git config --global core.editor "code --wait"

# Set default branch name
git config --global init.defaultBranch main

# Set credential helper
git config --global credential.helper store

# Set aliases
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.lg "log --oneline --graph --all"
```

### Repository-specific Configuration
```bash
# Set local user for specific repository
git config user.name "Your Name"
git config user.email "your.email@example.com"

# Set local editor
git config core.editor "vim"

# Set local ignore file
git config core.excludesfile .gitignore-local
```

## Troubleshooting

### Common Issues and Solutions
```bash
# Fix detached HEAD
git checkout main

# Recover deleted branch
git reflog
git checkout -b recovered-branch commit_hash

# Fix merge conflicts
git status  # See conflicted files
# Edit conflicted files manually
git add resolved_file
git commit -m "Resolve merge conflicts"

# Reset to remote state
git fetch origin
git reset --hard origin/main

# Clean up large files from history
git filter-branch --tree-filter 'rm -f large_file' HEAD
git push --force origin main
```

### Non-Fast Forward Issues
The "non-fast forward" error occurs when your local branch is behind the remote branch. Here are solutions:

#### Option 1: Fetch First (Recommended Approach)
```bash
# First, fetch to see what's on the remote
git fetch origin

# Check what's different
git log HEAD..origin/branch_name --oneline

# See the differences
git diff HEAD origin/branch_name

# Then decide how to proceed:
# Option A: Merge remote changes
git merge origin/branch_name

# Option B: Rebase your changes on top
git rebase origin/branch_name

# Option C: Reset to remote (loses local changes)
git reset --hard origin/branch_name
```

#### Option 2: Pull and Merge (Quick Fix)
```bash
# Get latest changes and merge them
git pull origin branch_name
# Resolve conflicts if any
git push origin branch_name
```

#### Option 3: Pull with Rebase
```bash
# Get latest changes and rebase your commits on top
git pull --rebase origin branch_name
# Resolve conflicts if any
git push origin branch_name
```

#### Option 4: Force Push (Use with Caution)
```bash
# Only use if you're sure you want to overwrite remote changes
git push --force origin branch_name

# Safer alternative - force push only if no one else has pushed
git push --force-with-lease origin branch_name
```

#### Option 5: Reset to Remote
```bash
# Completely reset your local branch to match remote
git fetch origin
git reset --hard origin/branch_name
# Your local changes will be lost!
```

#### Option 6: Create New Branch with Your Changes
```bash
# Save your changes in a new branch
git checkout -b backup-branch
git checkout main
git pull origin main
git checkout -b new-feature-branch
git cherry-pick backup-branch
git push origin new-feature-branch
```

#### Option 7: Handle Unrelated Histories
When you get "fatal: refusing to merge unrelated histories", use:
```bash
# Force merge of unrelated histories
git pull origin main --allow-unrelated-histories

# Resolve any conflicts, then commit
git add .
git commit -m "Merge unrelated histories"

# Push to remote
git push origin main
```

#### Option 8: Reset and Start Fresh
```bash
# Backup your current work
git checkout -b backup-branch

# Reset main to match remote
git checkout main
git fetch origin
git reset --hard origin/main

# Apply your changes on top
git cherry-pick backup-branch
# Or manually copy your changes back
```

## Best Practices

### Commit Messages
- Use conventional commits: `type(scope): description`
- Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`
- Keep subject line under 50 characters
- Use imperative mood ("add" not "added")

### Branch Naming
- `feature/description` - New features
- `bugfix/description` - Bug fixes
- `hotfix/description` - Urgent fixes
- `release/version` - Release preparation

### Repository Structure
- Keep `.gitignore` up to date
- Use meaningful commit messages
- Review changes before committing
- Use branches for features and fixes
- Keep main branch stable 