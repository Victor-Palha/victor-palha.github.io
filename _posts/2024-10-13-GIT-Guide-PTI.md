---
layout:
title: "The Beginner's Guide of GIT - What is Git, How to Install Git and Fundamental Commands"
date: 2024-10-13 18:14:20 -0000
categories: [Documentation]
tags: [guide, git]
---

# Git - Beginner's Guide

![Git Logo](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e0/Git-logo.svg/1280px-Git-logo.svg.png)

## What is Version Control?
* A technique that helps to **manage the source code** of an application;
* It records **all code modifications**, with the ability to **revert changes** when necessary;
* Allows the creation of software versions at different stages, making it **easy to switch between them**;
* Each team member can work on a different version simultaneously;
* There are tools for version control, such as **Git** and SVN.

## What is Git?
* The **most widely used** version control system in the world today;
* Git is based on **repositories**, which contain all versions of the code, as well as copies for each developer;
* All Git operations are **optimized for high performance**;
* Git ensures that all data is **secured with encryption** to prevent unauthorized or malicious changes;
* Git is an **open-source project**.

### More about Git:
* **Branching**: One of Git's powerful features is the ability to create branches, allowing developers to work on different features, fixes, or experiments without affecting the main codebase. Branches can be merged when ready, ensuring smooth collaboration.
* **Distributed System**: Git is a **distributed version control system**, meaning every developer has a complete copy of the repository on their local machine. This enables working offline and faster operations.
* **Staging Area**: Git includes a staging area (also called an index), where changes are reviewed and selected before committing them to the repository. This adds a layer of control over what gets tracked.
* **Collaboration**: Git integrates seamlessly with platforms like **GitHub, GitLab, and Bitbucket**, which provide hosting services for Git repositories, offering features like pull requests, code reviews, and project management tools.
* **Commit History**: Git keeps a detailed log of every change, known as commit history, allowing for transparency, accountability, and an easy way to track progress or identify issues.

## Git - Fundamental Commands

### What is a Repository?
* A repository is where the code is **stored**;
* Most projects have **one repository** for centralized code management;
* When we create a repository, we are essentially starting a project;
* The repository can be hosted on servers specialized in managing repositories, such as **GitHub** and **Bitbucket**;
* Each team member can download the repository and **create different versions** on their local machine.

### Additional Information:
* **Types of Repositories**: There are two main types of repositories in Git: local and remote. A **local repository** is on your machine, while a **remote repository** is hosted on a server, like GitHub. Changes are typically made locally and then pushed to the remote repository for sharing with the team.
* **Cloning a Repository**: When a developer starts working on a project, they usually **clone** the repository from a remote location to their local machine. This means they download the full history of the repository, including all branches, commits, and files.
* **Centralized vs. Distributed**: Git's distributed nature allows developers to work independently without relying on a central server at all times, unlike centralized version control systems, where all changes must go through a central repository.
* **Repository Management Platforms**: Services like **GitHub, GitLab, and Bitbucket** offer additional tools for repository management, such as issue tracking, continuous integration pipelines, and automated deployments, making them essential for team collaboration in modern development environments.

### Installing Git

To use Git, you need to first install it on your machine. Below are the steps for installing Git on different operating systems:

#### On Windows:
1. **Download Git**: Go to the official Git website and download the installer for Windows: [https://git-scm.com](https://git-scm.com).
2. **Run the Installer**: Run the downloaded installer and follow the setup instructions. During the installation, you can choose the default settings unless you need specific configurations.
3. **Verify Installation**:
   - Open **Git Bash** (a terminal that comes with the installation) or your Command Prompt.
   - Run the following command to verify that Git was installed successfully:
     ```bash
     git --version
     ```
   - If you see the Git version number, the installation was successful.

#### On macOS:
1. **Install via Homebrew (Recommended)**:
   - If you have Homebrew installed, you can install Git with the following command:
     ```bash
     brew install git
     ```
2. **Install via Xcode Command Line Tools**:
   - If you don’t have Homebrew, Git is also included as part of the Xcode Command Line Tools. To install it, open the terminal and run:
     ```bash
     xcode-select --install
     ```
3. **Verify Installation**:
   - After installation, check if Git was installed by running:
     ```bash
     git --version
     ```

#### On Linux:
1. **Install via Package Manager**:
   - On most Linux distributions, you can install Git directly from the default package manager.
   - For **Debian/Ubuntu** based systems:
     ```bash
     sudo apt update
     sudo apt install git
     ```
   - For **Fedora** based systems:
     ```bash
     sudo dnf install git
     ```
   - For **Arch Linux**:
     ```bash
    sudo pacman -S git
     ```
2. **Verify Installation**:
   - After installation, verify it by running:
     ```bash
     git --version
     ```

### Configuring Git After Installation
Once Git is installed, you should configure it with your user information so that your commits are properly labeled. Use the following commands to set your name and email:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

You can also check the configuration with:

```bash
git config --list
```

* **Upgrading Git**: On all platforms, you can regularly update Git using your system’s package manager or by downloading the latest version from the official website.
* **GUI Tools**: Along with Git, you might want to install graphical interfaces like **GitHub Desktop**, **Sourcetree**, or **GitKraken**, which provide a visual interface for Git, making it easier for beginners to manage repositories without using the command line.
* **SSH Setup**: If you plan to use Git with platforms like GitHub, setting up SSH authentication can simplify the process of pushing and pulling code. To generate an SSH key:
```bash
ssh-keygen -t rsa -b 4096 -C "your.email@example.com"
```

### Creating Repositories
* To create a repository, use the command: `git init`;
* This command will set up the necessary files to initialize the repository;
* These files are stored in the hidden folder **.git**;
* After this command, the current directory **will be recognized by Git as a project**, and Git will respond to further commands;
* To check the current status of the repository, you can use the command: `git status`.
* **.git Directory**: The `.git` folder contains all the metadata and history for your project. It stores information about commits, branches, tags, and more. This folder should not be modified manually, as it is critical to Git's functionality.
* **Initializing a Repository in an Existing Directory**: You can run `git init` inside any existing directory to turn it into a Git repository. This won't affect your files; it will simply allow Git to start tracking them. After running `git init`, you can begin adding files to the repository using:
    ```bash
    git add <file-name>
    ```
    Or to track all files in the directory:
    ```bash
    git add .
    ```
  
* **Git Status**: The `git status` command provides a **snapshot of the current state** of your repository. It shows which files are staged for the next commit, which are not being tracked, and which have been modified but not yet staged. This is an essential command for checking the progress of your work.

* **Skipping the Init Step**: If you are cloning an existing repository from a platform like GitHub, you won’t need to run `git init`, as the repository will already be initialized on your machine as part of the cloning process.

* **Using .gitignore**: After initializing a repository, it's a good idea to create a `.gitignore` file to specify which files or directories Git should ignore. This prevents unnecessary files like temporary files, build outputs, or sensitive data (e.g., API keys) from being tracked. Here’s an example `.gitignore`:
```gitignore
    # Ignore node_modules directory
    node_modules/
    
    # Ignore build files
    /build
```

### What is GitHub?
* GitHub is a **repository management service**, widely used and available for free;
* You can **upload your projects** to GitHub and make them available to other developers;
* GitHub is free for both public and **private** projects.

![github](https://i0.wp.com/puresourcecode.com/wp-content/uploads/2022/11/github-wallpaper-scaled.jpeg?fit=2560%2C1440&ssl=1)

* **Collaboration Platform**: GitHub is more than just a repository hosting service. It’s a platform that facilitates collaboration through features like **pull requests**, **code reviews**, and **issue tracking**, allowing teams to work together seamlessly.
  
* **GitHub Actions**: GitHub offers **CI/CD (Continuous Integration/Continuous Deployment)** tools through **GitHub Actions**, allowing you to automate tasks such as testing, building, and deploying your project whenever code is pushed or merged.
  
* **Forking and Open Source**: GitHub supports the open-source community by making it easy to **fork** a repository. Forking allows developers to create their own copy of a project, make changes, and contribute back by opening **pull requests**.
  
* **GitHub Pages**: One of the additional features GitHub offers is **GitHub Pages**, which allows you to host websites directly from a GitHub repository. It’s often used to host personal portfolios, project documentation, or static websites.

* **Integration with Tools**: GitHub integrates seamlessly with other tools and services, including project management software (like **Jira** and **Trello**) and continuous integration platforms, making it a central hub for many development workflows.

* **Social Features**: GitHub also has social elements. Developers can **star** repositories to show their support, **watch** projects to get updates, and **follow** other developers to stay informed about their activities. This makes it a valuable resource for learning and networking in the development community.

### Sending Repositories to GitHub
* We can easily **send our repositories** to GitHub;
* The process involves creating the project on GitHub, initializing it in Git on your machine, syncing it with GitHub, and pushing the code;
* Although this sequence might seem complex, it is easily done **with just a few commands**;
* It's important to note that this setup **is done only once per project**.

#### Step-by-Step Process:

1. **Create a Repository on GitHub**:
   - Log in to your GitHub account and click on the **"New Repository"** button.
   - Name your repository and choose whether it will be public or private.
   - Do not check the option to initialize the repository with a README, as you will initialize it locally.

2. **Initialize the Local Repository**:
   - On your local machine, navigate to the project folder where you want to initialize the repository.
   - Run the following command to initialize the local repository:
     ```bash
     git init
     ```

3. **Add and Commit Files**:
   - Add all your project files to the repository:
     ```bash
     git add .
     ```
   - Commit the changes with a descriptive message:
     ```bash
     git commit -m "Initial commit"
     ```

4. **Connect to GitHub**:
   - Link your local repository to the remote GitHub repository by adding the **remote origin**. Replace `<repository-url>` with the URL of your GitHub repository:
     ```bash
     git remote add origin <repository-url>
     ```

5. **Push to GitHub**:
   - Finally, push the changes from your local repository to GitHub using the following command:
     ```bash
     git push -u origin master
     ```
   - This command pushes the `master` branch (or `main`, depending on your default branch name) to the remote GitHub repository.

### Verifying Project Changes
* Project changes can be checked using the command: `git status`;
* This command is used **very frequently**;
* It maps out all changes in the project;
* It shows details such as **untracked files** and **modified files**;
* Essentially, it highlights the **differences** between the current state of your project and what has been already committed or pushed to the server.
* **Untracked Files**: These are files that exist in your project folder but have not yet been added to Git's tracking system. They are typically shown in red when you run `git status`.
  - To track them, you can use:
    ```bash
    git add <file>
    ```

* **Modified Files**: These are files that have been changed since the last commit. Git is aware of these files, but they are not yet staged for the next commit. They appear in the output of `git status` in a different color (usually yellow or green).
  - To stage modified files, use:
    ```bash
    git add <file>
    ```

* **Staging Area**: The `git status` command also tells you which files are in the **staging area**, waiting to be committed. Once files are staged, they are ready to be included in the next commit.
  
* **Tracking Changes**: After verifying changes with `git status`, you can further investigate them using `git diff` to see exactly what lines of code have been altered.
  ```bash
  git diff
  ```

* **Useful in Collaboration**: In team projects, running `git status` frequently ensures that you are aware of local changes before pulling updates from the remote repository. This prevents merge conflicts and keeps your local repository organized.

In summary, `git status` is essential for keeping track of your project's state and ensuring all changes are properly managed before committing and pushing updates.

### Adding Files to the Project
* To add new files to a project, we use the command: `git add`;
* You can add **a specific file** or **multiple files at once**;
* Only after adding files will they be tracked by Git;
* In other words, **if a file is not added**, it will not be under version control;
* It’s a good practice to use this command frequently to ensure no changes are left untracked.

#### Add a Specific File:
* Use the following command to add a single file:
  ```bash
  git add <file-name>
  ```

#### Add Multiple Files at Once:
* To add all files and changes in the project directory:
  ```bash
  git add .
  ```

#### Understanding `git add`
* **Staging Area**: When you run `git add`, you are moving files from your working directory to the **staging area** (also known as the index). Files in this area are queued to be included in the next commit.
  
* **Partial Staging**: You can also stage specific parts of a file by using `git add -p`, which allows you to review changes interactively and choose which parts to stage.

* **Untracked vs. Tracked Files**: Files that have never been added to Git are considered **untracked**. Once you use `git add`, these files become **tracked** and will be monitored for changes in future commits.

* **Removing Files from Staging**: If you accidentally add a file to the staging area but don’t want to include it in the next commit, you can remove it from the staging area with:
  ```bash
  git reset <file-name>
  ```

* **Adding Large Sets of Files**: If you're working on a project with many files, you can refine what to add by using patterns. For example, to add only `.js` files in a folder:
  ```bash
  git add *.js
  ```

By regularly using `git add`, you ensure that Git tracks the relevant changes and files in your project, helping maintain proper version control throughout your development process.

### Saving Project Changes
* Changes in the project are saved using the command: `git commit`;
* You can commit **specific files** or multiple files at once using the `-a` flag;
* It's good practice to provide **a message for each commit**, describing the changes made;
* The message can be added with the `-m` flag;
* Example:
  ```bash
  git commit -a -m "Commit message"
  ```

### Best Practices for Committing:
* **Staging and Committing**: Before committing, files must first be added to the staging area using `git add`. However, when using the `-a` flag, Git will automatically stage all modified files before committing (except new files, which still need `git add`).
  
* **Commit Messages**: Commit messages should be clear and descriptive, helping others (and your future self) understand what changes were made. It’s common practice to write messages in the imperative mood (e.g., "Fix bug" instead of "Fixed bug").
  
* **Best Practices for Commits**:
  - Make **small, frequent commits**: This helps keep changes organized and makes it easier to roll back if needed.
  - Keep the **commit message brief but informative**: A good rule of thumb is to summarize the change in the title and, if necessary, provide further explanation in the body of the message (for longer commit messages).
  
* **Committing Specific Files**: If you only want to commit a specific file or set of files, you can do so without using the `-a` flag:
  ```bash
  git commit <file-name> -m "Commit message"
  ```

* **Amending Commits**: If you realize you forgot to include a file or made a mistake in the message after committing, you can use the `--amend` flag to modify the most recent commit:
  ```bash
  git commit --amend -m "Updated commit message"
  ```

* **Skipping the Commit Message**: Although not recommended, you can commit without a message by using the `--allow-empty-message` flag, though it’s better to always include meaningful messages:
  ```bash
  git commit --allow-empty-message -m ""
  ```

### Sending Code to the Remote Repository
* When we finish a new feature, we **send the code to the repository**, which serves as the source code;
* This action is performed using the command: `git push`;
* After this action, **the server code will be updated based on the local code** that was sent.

### Receiving Changes
* It’s also common to need to **sync your local repository** with changes from the remote repository;
* This action is performed using: `git pull`;
* After running this command, it will **fetch updates** from the remote repository, and if any are found, they will be **merged with the existing code** on your machine.
* For this command to work, the remote repository must have files that the local repository does not contain. To test this functionality, you can create a file directly on GitHub.

#### Additional Information:
* **Understanding `git push`**:
  - The `git push` command uploads your local commits to the remote repository. It updates the remote branch with your changes.
  - You can specify which branch to push to by using:
    ```bash
    git push origin <branch-name>
    ```
  - If you are pushing to a new remote branch for the first time, you may need to set the upstream branch using:
    ```bash
    git push -u origin <branch-name>
    ```

* **Understanding `git pull`**:
  - The `git pull` command is a combination of `git fetch` (which retrieves the changes from the remote) and `git merge` (which merges those changes into your current branch).
  - You can specify which remote branch to pull from:
    ```bash
    git pull origin <branch-name>
    ```

* **Handling Merge Conflicts**: 
  - If you and someone else have made changes to the same line in a file, a **merge conflict** will occur during the `git pull`. You will need to resolve these conflicts manually before committing the changes.
  - Git will mark the conflicting areas in the file, and you'll need to edit them and then stage the resolved file:
    ```bash
    git add <resolved-file>
    git commit -m "Resolved merge conflict"
    ```

* **Checking Remote Repository Status**: 
  - Before pushing or pulling, you might want to check the status of your local branch against the remote branch. You can do this with:
    ```bash
    git status
    ```
  - It will show if your branch is ahead, behind, or has diverged from the remote branch.

By regularly using `git push` and `git pull`, you can ensure that your local repository stays in sync with the remote repository, allowing for smooth collaboration with other developers and the maintenance of an organized project history.

### Cloning Repositories
* The act of downloading a repository from a remote server is called **cloning a repository**;
* To perform this action, we use: `git clone <repository link>`;
* You need to provide the **reference** to the remote repository;
* The reference can be obtained from GitHub by clicking on the **Code** button within the repository;
* This command is commonly used when **starting a new project**, for example.

* **What Happens When You Clone**:
  - When you clone a repository, Git creates a local copy of the entire repository, including its history and all branches. This allows you to work on the project offline and make local changes.
  - The cloned repository is linked to the remote repository, allowing you to push changes back and pull updates easily.

* **Clone with SSH or HTTPS**:
  - You can clone a repository using either the **SSH** or **HTTPS** link. Using SSH is often preferred for secure authentication without needing to enter your username and password each time.
  - Example for SSH:
    ```bash
    git clone git@github.com:username/repo.git
    ```
  - Example for HTTPS:
    ```bash
    git clone https://github.com/username/repo.git
    ```

* **Cloning Specific Branches**:
  - If you want to clone a repository but only work on a specific branch, you can use the `-b` flag:
    ```bash
    git clone -b <branch-name> <repository link>
    ```

* **Using a Different Directory Name**:
  - By default, Git will create a directory with the same name as the repository. However, you can specify a different directory name by adding it to the end of the clone command:
    ```bash
    git clone <repository link> <custom-directory-name>
    ```

* **Updating Your Local Clone**:
  - Once you have cloned a repository, you can keep your local copy up-to-date with the remote repository using:
    ```bash
    git pull
    ```

### Removing Files from the Repository
* Files can be **deleted from Git's tracking** system;
* The command to delete a file is: `git rm <file>`;
* After deleting a file using this command, it will no longer have its updates considered by Git;
* The file can only be tracked again if it is added back using `git add`.

#### What Happens When You Delete a File:
* **Effects of `git rm`**:
  - When you run `git rm <file>`, the file is removed from both the working directory and the staging area, meaning it will be deleted from your local file system and marked for deletion in the next commit.
  
* **Deleting Files Without Removing from Disk**:
  - If you want to remove a file from Git's tracking without deleting it from your local file system, you can use the `--cached` option:
    ```bash
    git rm --cached <file>
    ```
  - This keeps the file in your working directory but stops tracking it in the repository.

* **Committing the Deletion**:
  - After using `git rm`, remember to commit the changes to finalize the deletion in the repository:
    ```bash
    git commit -m "Removed <file> from the repository"
    ```

* **Deleting Multiple Files**:
  - You can also delete multiple files at once by listing them:
    ```bash
    git rm <file1> <file2> <file3>
    ```
  - To delete all files matching a specific pattern, use:
    ```bash
    git rm *.log  # For example, to remove all .log files
    ```

* **Undoing Deletions**:
  - If you accidentally delete a file and have not yet committed the change, you can restore the file using:
    ```bash
    git checkout -- <file>
    ```
  - If you have already committed the deletion but want to restore the file, you can use:
    ```bash
    git checkout HEAD~1 -- <file>
    ```
  - This command checks out the version of the file from the previous commit.

### Change History
* You can **access a log** of modifications made to the project;
* The command for this feature is: `git log`;
* This command provides information about the **commits made** in the project up to that point.

#### Why Use `git log`?
* **Log Details**: When you run `git log`, you'll see a list of commits along with details such as:
  - **Commit Hash**: A unique identifier for each commit (a long alphanumeric string).
  - **Author**: The name and email of the person who made the commit.
  - **Date**: The date and time when the commit was made.
  - **Commit Message**: A brief description of the changes made in that commit.

* **Basic Usage**:
  ```bash
  git log
  ```

* **Customizing the Log Output**:
  - You can customize the output of `git log` using various options:
    - **One-line Format**: To view the log in a more compact format:
      ```bash
      git log --oneline
      ```
    - **Graph View**: To visualize the commit history as a graph:
      ```bash
      git log --graph --oneline --decorate
      ```
    - **Limit the Number of Entries**: To see a specific number of commits:
      ```bash
      git log -n 5  # Shows the last 5 commits
      ```

* **Viewing Changes in a Commit**:
  - To see the changes made in a specific commit, you can use:
    ```bash
    git show <commit-hash>
    ```
  - This command will display the changes, including the diff of what was modified.

* **Filtering Logs**:
  - You can filter commits by author, date, or commit message using:
    - By Author:
      ```bash
      git log --author="Author Name"
      ```
    - By Date:
      ```bash
      git log --since="2024-01-01" --until="2024-12-31"
      ```
    - By Keyword in Commit Message:
      ```bash
      git log --grep="fix"
      ```

* **Reverting to Previous Commits**:
  - If you need to revert your project to a previous commit, you can use:
    ```bash
    git checkout <commit-hash>
    ```
  - However, be cautious, as this will put your working directory in a "detached HEAD" state. To revert changes while preserving the history, consider using `git revert`.

Using `git log` effectively allows you to track the history of your project, making it easier to understand changes over time and facilitating better collaboration and code management.

### Undoing Changes
* A modified file can be **returned to its original state**;
* The command used for this is `git checkout <file-name>`;
* After executing this command, the file will exit the staging area;
* If further modifications are made after this, the file will re-enter the staging area again.

#### You really need to learn about undoing changes in Git... Trust me!
* **Understanding `git checkout <file>`**:
  - Running `git checkout <file>` will revert the specified file to its last committed state, effectively discarding any changes made since the last commit.
  - This action cannot be undone, so it’s crucial to ensure that you really want to discard the changes before using this command.

* **Alternative for Unstaging**:
  - If you simply want to unstage a file (remove it from the staging area without reverting changes), you can use:
    ```bash
    git reset <file>
    ```
  - This will keep the modifications in your working directory but remove the file from the staging area.

* **Restoring Entire Working Directory**:
  - To discard changes in all files and return the entire working directory to the last committed state, you can use:
    ```bash
    git checkout .
    ```
  - This command should be used with caution, as it will discard changes across all modified files.

* **Viewing Changes Before Undoing**:
  - If you want to see what changes will be discarded, you can use:
    ```bash
    git diff <file>
    ```
  - This will show you the differences between the current state of the file and the last committed version.

* **Recovering Deleted Files**:
  - If you accidentally deleted a file and committed the change, you can recover it using:
    ```bash
    git checkout HEAD~1 -- <file>
    ```
  - This retrieves the file from the previous commit.

* **Using `git restore`**:
  - In newer versions of Git, you can also use `git restore` to discard changes:
    ```bash
    git restore <file>
    ```
  - This command is specifically designed for restoring files and is often seen as more intuitive than `git checkout`.

### Undoing All Changes
* With the command `git reset`, we can reset all changes made;
* It is generally used with the **--hard** flag;
* All **committed changes** and **pending changes** will be deleted.
* Example: 
  ```bash
  git reset --hard <branch-name>
  ```
  or
  ```bash
  git reset --hard origin/main
  ```

#### Sometimes you need to reset all changes in Git... Here is how you can do it!
* **Understanding `git reset --hard`**:
  - The `git reset --hard` command resets the current branch to the specified commit, discarding all changes in the working directory and the staging area. This means that both tracked files and untracked files will be lost.
  - Use this command with extreme caution, as it cannot be undone easily. You will lose all uncommitted work permanently.

* **Resetting to a Specific Commit**:
  - If you want to reset your branch to a specific commit, you can do so by providing the commit hash:
    ```bash
    git reset --hard <commit-hash>
    ```

* **Common Use Cases**:
  - **Reverting to the Last Committed State**: If you've made numerous changes and decide that you want to revert to the last commit completely, you can use:
    ```bash
    git reset --hard HEAD
    ```
  - **Aligning with the Remote**: If you want to make your local branch match the state of a remote branch (e.g., `origin/main`), you would use:
    ```bash
    git reset --hard origin/main
    ```

* **Recovering Lost Changes**:
  - If you accidentally used `git reset --hard`, recovering lost changes can be challenging. However, you might be able to find your previous commits in the reflog:
    ```bash
    git reflog
    ```
  - The reflog shows a history of where your HEAD has been, allowing you to recover lost commits.

* **Safer Alternatives**:
  - If you are unsure about losing changes, consider using `git reset` without the `--hard` flag, which will keep changes in your working directory:
    ```bash
    git reset --soft <commit-hash>
    ```
  - This command will reset the branch to the specified commit but keep all changes staged, allowing you to review them before deciding what to do next.

* **Cleaning Up Untracked Files**:
  - If you want to remove untracked files after a hard reset, you can use:
    ```bash
    git clean -fd
    ```
  - This command will remove all untracked files and directories.

Using `git reset --hard` is a powerful way to revert changes, but it comes with risks. Always ensure that you have backups of important changes or consider alternative methods before using it.