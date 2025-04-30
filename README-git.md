# Git Workflow Guide

This guide covers common Git operations, emphasizing best practices, especially when working across multiple machines or collaborating. While command-line instructions are provided, using the Git integration within Visual Studio Code (VSCode) is often more intuitive and efficient for daily tasks.

## Git Operations in Visual Studio Code (VSCode)

VSCode offers a powerful and user-friendly interface for most Git operations, which is often the preferred method.

1.  **Open your repository in VSCode**:
    *   Navigate to your repository directory (`File > Open Folder...`) or open it from the command line (`code .`).

2.  **Access Source Control Panel**:
    *   Click the Source Control icon (looks like branching paths) in the Activity Bar on the left, or press `Ctrl+Shift+G`.

3.  **View Changes**:
    *   Modified files appear under "Changes". Untracked files are listed separately.

4.  **Stage Changes**:
    *   Hover over a file under "Changes" and click the `+` (Stage Changes) icon.
    *   To stage all changes, click the `+` icon next to the "Changes" heading.

5.  **Commit Changes**:
    *   Enter a descriptive commit message in the input box at the top of the Source Control panel.
    *   Click the checkmark icon (Commit) or press `Cmd+Enter` (macOS) / `Ctrl+Enter` (Windows/Linux).

6.  **Pull Changes (Fetch & Rebase/Merge)**:
    *   Click the `...` (Views and More Actions) menu in the Source Control panel.
    *   Select `Pull, Push > Pull` or `Pull, Push > Pull (Rebase)`. Using Rebase (`git pull --rebase`) is often recommended to maintain a cleaner history (see Best Practices).
    *   Alternatively, click the Synchronize Changes button (circular arrows) in the status bar (bottom left), which typically performs a pull followed by a push.

7.  **Push Changes**:
    *   After committing, click the `...` menu and select `Pull, Push > Push`.
    *   Or use the Synchronize Changes button in the status bar.

8.  **Switch Branches**:
    *   Click the current branch name in the bottom-left corner of the VSCode window.
    *   Select the desired branch from the dropdown list.

9.  **Create a New Branch**:
    *   Click the current branch name in the bottom-left corner.
    *   Select `+ Create new branch...`.
    *   Enter the name for your new branch and press Enter.

10. **Merge Branches**:
    *   Ensure you are on the branch you want to merge *into* (e.g., `main`). Switch to it if necessary (see "Switch Branches").
    *   Click the `...` menu in the Source Control panel.
    *   Select `Branch > Merge Branch...`.
    *   Choose the branch you want to merge *from* (e.g., `your-feature-branch`).
    *   VSCode will perform the merge. Resolve any conflicts if prompted.

11. **Resolve Merge Conflicts**:
    *   VSCode highlights conflicting files in the Source Control panel.
    *   Open the conflicting file. VSCode provides inline tools to `Accept Current Change`, `Accept Incoming Change`, `Accept Both Changes`, or `Compare Changes`.
    *   Manually edit the file to resolve the conflict if needed, removing the `<<<<<<<`, `=======`, and `>>>>>>>` markers.
    *   Once resolved, stage the file by clicking the `+` icon next to it in the Source Control panel.
    *   After resolving all conflicts, commit the merge by entering a commit message and clicking the checkmark icon.

## Initial Repository Setup (Command Line)

These commands are typically used only once when first setting up a project locally or connecting it to a remote repository.

### Situation: Cloning an Existing Remote Repository

This is the most common way to start working on an existing project hosted on platforms like GitHub.

```bash
# Clone a repository from a remote URL (like GitHub)
# This creates a local copy in a new directory named after the repo
git clone https://github.com/your-username/your-repo.git

# Navigate into the newly cloned directory
cd your-repo

# The remote named 'origin' is automatically configured to point to the URL you cloned from.
# Your local 'main' branch (or default branch) is usually set up to track 'origin/main'.
```

### Situation: Starting a New Project Locally and Connecting to a New Remote

Use this when you have created a new, empty repository on a platform like GitHub and want to push your local project to it for the first time.

```bash
# Navigate to your project directory
cd /path/to/your/project

# Initialize a new Git repository locally
git init -b main # Initializes with 'main' as the default branch

# Stage all your initial project files
git add .

# Make your first commit
git commit -m "Initial commit"

# Add the remote repository URL (replace with your actual URL from GitHub/etc.)
# 'origin' is the conventional name for the primary remote repository
git remote add origin https://github.com/your-username/your-new-repo.git

# Verify the remote was added
git remote -v

# Push the initial commit from your local 'main' branch to the remote 'origin' repository
# The -u flag sets the upstream tracking relationship, so future 'git pull' and 'git push'
# from the 'main' branch will automatically target 'origin/main'
git push -u origin main
```

### Situation: Connecting an Existing Local Project to an Existing Remote History

Use this scenario carefully. It's for when you have local project files that correspond to a repository already existing on a remote (e.g., you manually downloaded files or initialized git locally before realizing a remote exists), and you want to sync your local version with the remote's history.

```bash
# Navigate to your existing project directory
cd /path/to/existing/project

# Initialize Git if not already done
git init -b main

# Add the remote repository URL
git remote add origin https://github.com/your-username/existing-remote-repo.git

# Fetch the history (commits, branches) from the remote repository
git fetch origin

# Option 1: Discard local changes and match the remote (SAFER if local work isn't needed)
# WARNING: This overwrites your local files and history to match the remote branch!
# Ensure you have backed up any essential local-only changes first.
git reset --hard origin/main # Or origin/master if that's the remote default branch name

# Option 2: Keep local work by creating a new branch based on the remote state
# This is useful if you have local commits you want to integrate later.
# git checkout -b my-local-work origin/main # Creates 'my-local-work' branch from remote 'main'
# Now you can compare 'my-local-work' with the original local state or merge changes carefully.

# After resetting or branching, your local repo is now linked to the remote.
# You can proceed with the daily workflow (pull, push, etc.).
```

## Daily Workflow (Command Line)

These are the commands you'll use most frequently during development.

### Situation: Starting Work or Syncing Local with Remote

Always do this before starting new work or after pulling changes on another machine.

```bash
# Navigate to your project directory
cd /path/to/project

# Ensure you are on the correct branch (e.g., main or a feature branch)
git checkout main # or git checkout your-feature-branch

# Fetch all changes and branch information from the remote named 'origin'
# This updates your local knowledge of the remote but doesn't change your files yet.
git fetch origin

# Integrate remote changes into your local working branch.
# Using --rebase is generally recommended for cleaner history, especially on feature branches
# or if you haven't pushed your local commits yet. It replays your local commits on top
# of the fetched remote changes.
git pull --rebase origin main # Pulls from 'origin/main' and rebases your local 'main'

# If you prefer merging (creates a merge commit, can clutter history on shared branches):
# git pull origin main

# Now your local branch is up-to-date with the remote.
```

### Situation: Making and Saving Changes Locally

This is the core loop: edit, stage, commit.

```bash
# 1. Modify your project files using your editor.

# 2. Check the status of your changes (shows modified, new, deleted files)
git status

# 3. Stage changes you want to include in the next commit
# Stage specific files:
git add path/to/your/file.txt path/to/another/file.py
# Stage all modified and new files in the current directory and subdirectories:
git add .
# Stage changes interactively (lets you choose parts of files):
# git add -p

# 4. Commit the staged changes with a descriptive message
git commit -m "Implement user authentication feature"
# For longer messages, omit -m to open your default editor:
# git commit
```

### Situation: Sharing Your Local Commits with the Remote

After committing changes locally, push them to the remote repository so others (or your other machines) can see them.

```bash
# Push your committed changes from your local branch to the corresponding remote branch
# Assumes your local branch is tracking the remote branch (e.g., set up via 'git push -u' or 'git clone')
git push origin your-branch-name # e.g., git push origin main or git push origin feature/login

# If the remote branch doesn't exist or tracking isn't set up, you might need:
# git push --set-upstream origin your-branch-name # or git push -u origin your-branch-name
```

## Working with Branches (Command Line)

Branches allow you to work on features or fixes in isolation without affecting the main codebase.

### Situation: Creating and Switching to a New Feature Branch

Standard practice for developing new features or fixing bugs.

```bash
# Ensure you are starting from an up-to-date base branch (e.g., main)
git checkout main
git pull --rebase origin main

# Create a new branch named 'new-feature-branch' and switch to it immediately
git checkout -b new-feature-branch

# Now you are on the new branch. Make your changes, stage, and commit as usual.
# git add .
# git commit -m "Start working on new feature X"
# git push -u origin new-feature-branch # Push the new branch to remote for the first time
```

### Situation: Switching Between Existing Branches

```bash
# Switch back to the main branch
git checkout main

# Switch to an existing feature branch
git checkout feature/existing-feature
```

### Situation: Merging a Feature Branch into the Main Branch

Once a feature is complete and tested on its branch, merge it back into the main line (e.g., `main`).

```bash
# 1. Switch to the branch you want to merge INTO (the target branch)
git checkout main

# 2. Ensure the target branch is up-to-date with the remote
git pull --rebase origin main

# 3. Merge the feature branch INTO the current branch (main)
git merge new-feature-branch
# Git will attempt to automatically merge the changes.

# 4. Resolve conflicts if they occur (see Conflict Resolution section)

# 5. Commit the merge (often automatic if no conflicts, or after resolving conflicts)
# git commit -m "Merge branch 'new-feature-branch'" # Git usually suggests a message

# 6. Push the updated main branch (with the merged changes) to the remote
git push origin main
```

### Situation: Deleting Branches After Merging

Clean up branches that are no longer needed.

```bash
# Delete the local branch (use -d for safety, checks if merged; -D forces delete)
git branch -d new-feature-branch

# Delete the remote branch
git push origin --delete new-feature-branch
```

## Specific Scenarios & Troubleshooting (Command Line)

Commands for handling less common situations or fixing problems.

### Situation: Resolving Merge Conflicts

Conflicts happen when Git can't automatically merge changes from different sources (e.g., during `git pull`, `git merge`, `git rebase`).

```bash
# Git will notify you of conflicts during pull/rebase/merge and mark conflicted files.
# Check which files have conflicts:
git status
```

```markdown
# Manually resolve conflicts:
# 1. Open each conflicted file in your editor.
# 2. Look for the conflict markers added by Git:
#    <<<<<<< HEAD
#    (Code from your current branch/HEAD)
#    =======
#    (Code from the incoming branch/commit)
#    >>>>>>> [commit hash or branch name]
# 3. Edit the file: Remove the markers and keep the correct combination of code.
```

```bash
# After resolving conflicts in a file, stage it to mark it as resolved:
git add path/to/resolved/file.txt
```

```markdown
# Continue the process that caused the conflict:
# If during a rebase:
```

```bash
git rebase --continue
```

```markdown
# If during a merge:
# After staging all resolved files, commit the merge:
```

```bash
git commit # Git often pre-fills the commit message for merges
```

### Situation: Undoing Local Changes

*   **Discard changes in working directory (before staging/committing):**

    ```bash
    # Discard changes in a specific file since the last commit
    git checkout -- path/to/file.txt
    # Discard ALL changes in the current directory since last commit (DANGEROUS!)
    git checkout -- .
    ```

*   **Unstage files (remove from staging area, before committing):**

    ```bash
    # Unstage a specific file, keeping changes in the working directory
    git reset HEAD path/to/file.txt
    # Unstage all staged files
    git reset HEAD .
    ```

*   **Modify the most recent commit (before pushing):**

    ```bash
    # Add more changes to the last commit
    git add forgotten_file.txt
    # Amend the last commit: combines staged changes and/or lets you edit the message
    git commit --amend
    # To only change the last commit message:
    git commit --amend -m "New, corrected commit message"
    # WARNING: Avoid amending commits that have already been pushed to a shared remote.
    ```

*   **Revert a previous commit (safe, creates a new commit that undoes changes):**

    ```bash
    # Find the hash of the commit you want to undo (e.g., using 'git log')
    git log
    # Create a new commit that reverses the changes introduced by <commit-hash>
    git revert <commit-hash>
    # Push the new revert commit
    git push origin your-branch-name
    ```

*   **Reset to a previous state (dangerous, rewrites history):**

    ```bash
    # WARNING: Use with extreme caution, especially on shared branches. Rewrites history.
    # Find the hash of the commit you want to reset TO.
    git log
    # Option 1: Reset history, keep changes from discarded commits in working dir (--soft is similar but keeps them staged)
    git reset <commit-hash>
    # Option 2: Reset history AND discard all changes from discarded commits (DANGEROUS!)
    git reset --hard <commit-hash>
    # If you've pushed the commits you're discarding, you'll need to force push (`git push --force-with-lease`),
    # which is risky for collaboration. Generally, prefer `git revert` for pushed commits.
    ```

### Situation: Using Git Crypt for Sensitive Files

Ensure `git-crypt` is installed (`brew install git-crypt` on macOS).

```bash
# Navigate to the repo root
cd /path/to/your/repo

# Initialize git-crypt (only needs to be done once per repo)
# git-crypt init

# Tell git-crypt which files to encrypt via the .gitattributes file
# Create or edit .gitattributes and add lines like:
# secret_file filter=git-crypt diff=git-crypt
# *.key filter=git-crypt diff=git-crypt
# Add and commit .gitattributes
# git add .gitattributes
# git commit -m "Configure git-crypt"

# Add collaborators (optional, uses GPG keys)
# git-crypt add-gpg-user USER_ID

# Export the symmetric key (do this once and back it up SECURELY OFFLINE)
# This key is needed to unlock the repo on a new machine if not using GPG.
# git-crypt export-key /path/to/secure/backup/repository_name.key

# Unlock an existing encrypted repo on a new machine (if using symmetric key)
# Place the backed-up key file somewhere accessible
git-crypt unlock /path/to/your/repository_name.key
# Files specified in .gitattributes will now be decrypted locally.
# They remain encrypted in the Git history and on the remote.
```

### Situation: Fixing Broken Symbolic Links

Symbolic links in a repo can sometimes break or turn into plain files, especially when cloning across different operating systems or using certain tools.

```bash
# Example: Recreate specific symbolic links if they became regular files
# Ensure you are in the correct directory within the repo
cd /path/to/repo/subdirectory

# Remove the broken file/link first if it exists as a regular file
rm my_link.md

# Create the correct symbolic link (adjust paths as necessary)
# ln -s <target_file_or_directory> <link_name>
ln -s ../../path/to/target/file.txt my_link.md

# Stage the change (Git tracks the link itself)
git add my_link.md
git commit -m "Fix broken symbolic link my_link.md"
```

## Working with Multiple Machines / Collaboration

Strategies for keeping repositories synchronized when working on more than one computer or with other people.

### Important Note on Cloud-Synced Repositories (iCloud, OneDrive, Dropbox, etc.)

**Using a single local Git repository stored directly within a cloud-synced folder (like iCloud Drive, OneDrive, Dropbox) and accessing it from multiple machines simultaneously is STRONGLY DISCOURAGED.**

*   **Risk of Corruption:** Cloud sync services are not designed for the rapid, complex file operations Git performs within the `.git` directory. Sync conflicts or partial updates can easily corrupt your repository, leading to data loss or history loss.
*   **Interference:** Syncing can lock files Git needs, causing commands to fail unpredictably.
*   **Inefficiency:** Syncing thousands of small object files in `.git` is inefficient for cloud services.
*   **Accidental Commits:** Sync services might add their own metadata files (e.g., `.icloud`, conflict files) which can be accidentally committed.

**The Correct Approach:**

1.  **Central Remote Repository:** Use a dedicated Git hosting service (GitHub, GitLab, Bitbucket, etc.) as the central source of truth.
2.  **Separate Local Clones:** On *each* machine you use, `git clone` the repository from the central remote into a folder that is **NOT** managed by a cloud sync service (e.g., `~/Projects/`, `~/dev/`, `C:\Users\YourUser\Projects\`).
3.  **Sync via Git Commands:** Use `git pull` (preferably `git pull --rebase`) and `git push` to exchange changes between your local clones and the central remote repository.

**In summary: Treat the remote repository as the single source of truth. Use `git push` and `git pull` to coordinate changes between separate local clones on different machines. Do NOT rely on file synchronization services to manage or share a single local repository instance.**

### Tips for working with a *non-shared* repo in iCloud (Use with Extreme Caution)

If you absolutely *must* store a repository that you *only access from one machine at a time* within an iCloud Drive folder (still not recommended), ensure iCloud Drive's "Optimize Mac Storage" feature is turned OFF for that folder, or manually use "Download Now" on the entire project folder before working on it. Even then, the risk of issues remains higher than storing it outside synced folders. The multi-machine workflow below is far safer.

### General Multi-Machine Workflow (The Recommended Way)

This assumes you have separate clones of the same remote repository on each machine, stored outside cloud-synced folders.

1.  **Before Finishing Work on Machine A:**
    *   Commit all your local changes:

        ```bash
        git status # Check what needs committing
        git add . # Or add specific files
        git commit -m "Finished implementing feature Y"
        ```

    *   Push the changes to the remote repository:

        ```bash
        git push origin your-branch-name # e.g., git push origin main
        ```

2.  **When Starting Work on Machine B:**
    *   Navigate to the project directory on Machine B:

        ```bash
        cd /path/to/project/on/machine/b
        ```

    *   Switch to the correct branch if necessary:

        ```bash
        git checkout your-branch-name
        ```

    *   Fetch and integrate the latest changes pushed from Machine A (or by collaborators):

        ```bash
        git pull --rebase origin your-branch-name # Recommended
        # or git pull origin your-branch-name
        ```

    *   Now Machine B's local repository is up-to-date, and you can start working. Repeat the process when switching back.

## General Best Practices & Configuration

Tips for maintaining a clean and effective Git workflow.

*   **Commit Frequently:** Make small, atomic commits that represent a single logical change. This makes history easier to understand and revert if needed.
*   **Write Good Commit Messages:**
    *   Use the imperative mood in the subject line (e.g., "Fix bug", "Add feature", "Refactor module").
    *   Keep the subject line concise (ideally <= 50 characters).
    *   If more detail is needed, add a blank line after the subject and write a more detailed explanation in the body.
*   **Use Feature Branches:** Avoid working directly on the `main` (or `master`) branch for anything other than trivial fixes. Create descriptive branches for features, bugs, etc. (`git checkout -b feature/user-login`, `git checkout -b fix/bug-123`).
*   **Pull Before Pushing:** On shared branches (like `main`), always run `git pull --rebase` (preferred) or `git pull` immediately before `git push` to integrate any changes made by others first. This minimizes complex merge conflicts.
*   **Understand `.gitignore`:** Use a `.gitignore` file at the root of your repository to tell Git which files or directories it should ignore (e.g., compiled code, dependencies, log files, OS-specific files, editor configuration). Add `.gitignore` itself to the repository.
*   **Configure Git Globally:** Set up your identity and some helpful defaults.

        # Set your name and email (used in commit logs)
        git config --global user.name "Your Name"
        git config --global user.email "your.email@example.com"

        # Recommended: Make 'git pull' use rebase by default instead of merge
        git config --global pull.rebase true

        # Optional: Set the default branch name for new repositories created with 'git init'
        # git config --global init.defaultBranch main

        # Optional: Set your preferred editor for commit messages etc.
        # git config --global core.editor "code --wait" # Example for VSCode
