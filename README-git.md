# Initialization when Switching Projects or Laptops

## fetch remote changes
git fetch origin

## Pull changes frequently before starting new work or pushing, especially on shared branches.
git pull --rebase origin main

## encryption of void repo with git
cd /Users/markmorris/vibe/void
git-crypt export-key /Users/markmorris/vibe/void.key
git-crypt unlock ../void.key


## Create feature branches for development:
git checkout -b new-feature-branch


## if README links get broken
void:ln -s /Users/marmorri/vibe/my-globals/README-ask.md README-ask.md
void:ln -s /Users/marmorri/vibe/my-globals/README-prompts.md README-prompts.md


## don't do this with my_globals (and others?)  because it will break symbolic links
## starting with a fresh repo in /Users/markmorris/vibe/
git clone https://github.com/jmarkmorris/void.git
git clone https://github.com/jmarkmorris/run-aider.git


# Git Synchronization Workflow for Multiple Machines

## Important Note on Cloud-Synced Repositories (iCloud, OneDrive, etc.)

Using a single local Git repository stored in a cloud-synced folder (like iCloud Drive or OneDrive) and accessing it from multiple machines is **strongly discouraged** due to significant risks.

**Why it's Risky:**

*   **Git is Not Designed for Concurrent File Sync Access:** Git expects exclusive control over its internal files (`.git` directory). Cloud sync services operating on the same `.git` directory from multiple machines simultaneously can interfere with Git's operations, leading to corruption.
*   **Non-Atomic Sync:** Cloud sync processes are not instantaneous or atomic. Changes made on one machine might only be partially synced when accessed by another, leading to an inconsistent repository state and potential corruption or unpredictable behavior.
*   **Repository Corruption:** The most significant risk is corrupting your Git repository, potentially losing history or making it unusable.
*   **Unintended File Additions:** Cloud services might create temporary or conflict files (e.g., `.icloud` files) within the repository directory, which could be accidentally staged and committed.
*   **Performance Issues:** Syncing the numerous small files within the `.git` directory can cause performance degradation and high CPU usage.

**Recommended Practice:**

1.  **Separate Clones:** Clone the remote repository (e.g., from GitHub) independently onto each machine you use.
2.  **Store Repos Outside Synced Folders:** Keep your local Git repositories in directories that are *not* managed or synced by cloud services (e.g., `~/Projects/` or `~/vibe/` instead of `~/Documents/` if Documents are synced).
3.  **Sync via Git:** Use the standard Git workflow (`git pull`, `git push`) to synchronize changes between your local repositories and the central remote repository.

**In summary: Treat the remote repository (GitHub, GitLab, etc.) as the source of truth and use `git push` and `git pull` to coordinate changes between separate local clones on different machines. Do not rely on file synchronization services to manage a single shared local repository.**

## Tips for working with a *not* shared repo in iCloud

   - Use iCloud "keep downloaded" for the relevant project folders.

## Pre-Work Setup

1. **Use a Centralized Repository**

   - Host your project on GitHub, GitLab, or Bitbucket
   - Always treat these remote repositories as the "source of truth"

2. **Global Git Configuration**

   ```bash
   # Set up global git config on both machines
   git config --global user.name "Your Name"
   git config --global user.email "your.email@example.com"

   # Recommended: Set default branch behavior
   git config --global pull.rebase true
   ```

## Workflow Steps

### Before Switching Machines

1. **Always Commit Changes**

   ```bash
   # Stage all changes
   git add .

   # Commit with a descriptive message
   git commit -m "Detailed description of changes"

   # Push to remote repository
   git push origin [branch-name]
   ```

2. **Pull Latest Changes to New Local Repo**

# Initialize a Local (Remote) Repo

* git clone https://github.com/ my-user-name/my-repo.git

# Refresh a Local (Remote) Repo

   ```bash
   # Navigate to project directory
   cd /path/to/project

   # Fetch all remote changes
   git fetch origin

   # Pull and rebase current branch
   git pull --rebase origin [branch-name]
   # Also ensure your local main/master is up-to-date if needed
   # git checkout main
   # git pull --rebase origin main
   # git checkout [your-branch-name]
   ```

## Conflict Resolution Strategies

**If Conflicts Occur During Pull/Rebase**

   ```bash
   # Git will notify you of conflicts
   git status

   # Manually resolve conflicts in affected files
   # Open files, look for conflict markers (<<<<<<<, =======, >>>>>>>)
   # Edit the files to keep the desired changes and remove the markers

   # After resolving, stage the resolved files
   git add [conflicted-files]

   # Continue the rebase process
   git rebase --continue
   # Or if it was a merge conflict (less common with pull.rebase true):
   # git commit
   ```

## Best Practices

- Never work directly on the main/master branch for significant changes.
- Create feature branches for development (`git checkout -b new-feature-branch`).
- Use meaningful, descriptive commit messages.
- Commit small, logical chunks of work frequently.
- Pull changes frequently (`git pull --rebase`) before starting new work or pushing, especially on shared branches.

## Steps I took to initiate a neoclassical.ai repo on github from a local directory.

   ```bash
   # Example: Copying existing project files (excluding .git)
   # cp -r /path/to/old/project/ /path/to/new/neoclassical.ai/
   # cd /path/to/new/neoclassical.ai/

   # Initialize a new Git repository
   git init

   # Add the remote repository URL
   git remote add origin https://github.com/jmarkmorris/neoclassical.ai

   # Fetch the history from the remote
   git fetch origin

   # Reset local state to match the remote master/main branch
   # WARNING: This discards local changes not yet committed/pushed!
   git reset --hard origin/master # Or origin/main if that's the default

   # Optionally, create and switch to a new branch
   # git checkout -b my-working-branch
   ```

   ---

To initiate a new branch from the 'main' repository, pull down the latest versions of the repository, and push changes, you can follow these steps both in raw Git commands and in Visual Studio Code (VSCode).

### Raw Git Commands

1. **Navigate to your repository directory**:
    ```sh
    cd /path/to/your/repo
    ```

2. **Ensure you are on the 'main' branch**:
    ```sh
    git checkout main
    ```

3. **Pull the latest changes from the remote repository**:
    ```sh
    git pull origin main
    ```

4. **Create a new branch**:
    ```sh
    git checkout -b new-branch-name
    ```

5. **Make changes and commit them**:
    ```sh
    git add .
    git commit -m "Your commit message"
    ```

6. **Push the new branch to the remote repository**:
    ```sh
    git push origin new-branch-name
    ```

### Process in Visual Studio Code (VSCode)

1. **Open your repository in VSCode**:
    - Navigate to your repository directory and open it in VSCode.

2. **Open the Source Control panel**:
    - Click on the Source Control icon in the sidebar or press `Ctrl+Shift+G`.

3. **Ensure you are on the 'main' branch**:
    - Click on the branch name in the bottom-left corner of the VSCode window and select 'main' if you are not already on it.

4. **Pull the latest changes**:
    - Open the terminal in VSCode (`Ctrl+` or `View > Terminal`) and run:
      ```sh
      git pull origin main
      ```

5. **Create a new branch**:
    - Click on the branch name in the bottom-left corner again and select `Create new branch`.
    - Enter the name of your new branch.

6. **Make changes and commit them**:
    - Make your changes in the code editor.
    - Go to the Source Control panel, stage your changes by clicking the `+` icon next to the files you modified.
    - Enter your commit message in the input box at the top of the Source Control panel and click the checkmark icon to commit.

7. **Push the new branch to the remote repository**:
    - Open the terminal in VSCode and run:
      ```sh
      git push origin new-branch-name
      ```

To merge your changes back into the main repository, you can follow these steps both in raw Git commands and in Visual Studio Code (VSCode).

### Raw Git Commands

1. **Ensure you are on your feature branch**:
    ```sh
    git checkout new-branch-name
    ```

2. **Pull the latest changes from the remote repository to ensure your branch is up-to-date**:
    ```sh
    git pull origin new-branch-name
    ```

3. **Switch to the 'main' branch**:
    ```sh
    git checkout main
    ```

4. **Pull the latest changes from the remote 'main' branch**:
    ```sh
    git pull origin main
    ```

5. **Merge your feature branch into the 'main' branch**:
    ```sh
    git merge new-branch-name
    ```

6. **Resolve any merge conflicts if they arise**:
    - Open the conflicting files, resolve the conflicts, and then stage the resolved files:
      ```sh
      git add .
      ```

7. **Commit the merge**:
    ```sh
    git commit -m "Merged new-branch-name into main"
    ```

8. **Push the updated 'main' branch to the remote repository**:
    ```sh
    git push origin main
    ```

### Process in Visual Studio Code (VSCode)

1. **Ensure you are on your feature branch**:
    - Click on the branch name in the bottom-left corner of the VSCode window and select your feature branch (`new-branch-name`).

2. **Pull the latest changes**:
    - Open the terminal in VSCode (`Ctrl+` or `View > Terminal`) and run:
      ```sh
      git pull origin new-branch-name
      ```

3. **Switch to the 'main' branch**:
    - Click on the branch name in the bottom-left corner again and select `main`.

4. **Pull the latest changes from the remote 'main' branch**:
    - Open the terminal in VSCode and run:
      ```sh
      git pull origin main
      ```

5. **Merge your feature branch into the 'main' branch**:
    - Open the terminal in VSCode and run:
      ```sh
      git merge new-branch-name
      ```

6. **Resolve any merge conflicts if they arise**:
    - VSCode will highlight the conflicting files. Open each file, resolve the conflicts, and then stage the resolved files in the Source Control panel.

7. **Commit the merge**:
    - Enter your commit message in the input box at the top of the Source Control panel and click the checkmark icon to commit.

8. **Push the updated 'main' branch to the remote repository**:
    - Open the terminal in VSCode and run:
      ```sh
      git push origin main
      ```
