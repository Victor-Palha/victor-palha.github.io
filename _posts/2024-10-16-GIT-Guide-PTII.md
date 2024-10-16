---
layout:
title: "The Beginner's Guide of GIT - Branches, Repository Administration, Inspection and Commit Improvement"
date: 2024-10-16 09:04:20 -0000
categories: [Documentation]
tags: [guide, git]
---

# Branches
## What is a branch?
* A branch in Git is the way to **separate different versions of a project** within a repository;
* When a project is created, it starts on the **main** branch, and you're usually working on it at first;
* Typically, each new feature of a project is **developed on a separate branch**;
* After the changes are finalized, the branches are **merged** to integrate the new code into the final codebase;
* The **main** branch is the **primary** branch of the project, where the final version of the code is stored.

## Creating and viewing branches
* To see all available branches, use the command: `git branch`;
* To create a new branch, use the command: `git branch <branch-name>`;
* These two operations are frequently used in a developer’s daily work.

## Deleting branches
* You can delete a branch using the `-d` or `--delete` flag;
* **It’s not common to delete a branch**, as it’s usually better to keep the history of the work;
* Deleting is generally done if a branch was created by mistake;
* The command to delete a branch is: `git branch -d <branch-name>`.

## Switching between branches
* You can switch to another branch using: `git checkout <branch-name>`;
* To create and immediately switch to a new branch, use: `git checkout -b <branch-name>`;
* This command can also be used to discard changes in a file;
* When switching branches, you can carry over uncommitted changes, so **be cautious**.

## Merging branches
* The code from two separate branches can be merged using: `git merge <branch-name>`;
* This is another command that is **frequently used**;
* It's typically used to receive updates from other developers;
* The merge is performed on the branch you are currently on;
* A merge can be done either automatically or manually, depending on the complexity of the code;
* Usually, merges are managed through **pull requests** in remote repositories.

### Important considerations
* **Branch management** is crucial in collaborative environments, as it helps maintain cleaner project organization and avoid conflicting changes.
* It’s important to follow branch naming conventions, such as using descriptive names (e.g., `feature/add-login`, `bugfix/fix-crash`).
* Pull requests allow for **code review**, ensuring better quality and minimizing errors before merging code into the main branch.

# Stash
* We can save the current modifications **to switch to a different approach** without losing the current code;
* The command to perform this action is: `git stash`;
* After the command is executed, the branch will be reset to the version from the repository, clearing any uncommitted changes.

## Retrieving stash
* To view the stashes you’ve created, use the command: `git stash list`;
* You can retrieve a specific stash with the command: `git stash apply <number>`;
* To see the changes in a stash without applying it, use: `git stash show -p <number>`;
* This way, you can resume your work from where you left off by applying the stashed changes.

## Removing a stash
* To clear all stashes from a branch, use the command: `git stash clear`;
* If you need to delete a specific stash, you can do so with: `git stash drop <number>`.

### Important considerations
* Stashing is particularly useful when you need to quickly switch branches or work on a critical bug fix without losing your current progress.
* By default, `git stash` saves both tracked and modified files. If you also want to stash untracked files, use `git stash -u`.
* You can apply a stash and delete it in one step using `git stash pop`, which retrieves the stash and removes it from the list.
* Stashing keeps your working directory clean, preventing incomplete or experimental changes from interfering with your primary development flow.

# Tags
* You can create tags on branches using the command: `git tag -a <name> -m "<msg>"`;
* A tag is different from a stash; it serves as a **checkpoint for a branch**;
* Tags are used to mark important stages of development for a feature;
* To view all tags, use the command: `git tag`.

## Checking and changing tags
* To check a specific tag, use: `git show <name>`;
* You can switch between tags using: `git checkout <tag-name>`;
* This allows you to move backward or forward to different checkpoints in a branch;
* To remove a tag, use the command: `git tag -d <tag-name>`.

## Pushing and sharing tags
* Tags can be **pushed to the repository**, making them available to other developers;
* To push a specific tag, use: `git push origin <name>`;
* If you want to push multiple tags at once: `git push origin --tags`.

### Important considerations
* Tags are commonly used to mark **releases**, such as `v1.0`, helping teams easily reference specific versions of the code.
* Annotated tags (`-a`) include a message and metadata, while lightweight tags can be created without the `-a` flag if you don't need extra information.
* Tags do not move like branches—they are static points in the project history.

# Sharing and Updating

## Finding Branches
* New branches are created constantly, and **your local Git might not have mapped them** yet;
* To update your local repository with all new branches and tags that aren’t yet recognized, use: `git fetch -a`;
* This command is useful when you want to work on a branch created by another developer on your team, for example;
* A branch might not appear directly when using `git branch`, so you’ll need to switch to it using: `git checkout <branch-name>`.

## Receiving Updates
* The `git pull` command allows you to fetch updates from the remote repository;
* Each branch can be updated individually with `git pull`;
* It’s used to update the **main branch** or to get updates from another developer when working together on the same branch.

## Pushing Changes
* The `git push` command does the opposite of `pull`—it **sends your changes** to the remote repository;
* It’s also used to **share updates from a specific branch** with other developers;
* You’ll push changes when you’ve finished a task and need to send it to the repository.

## Using Remote
* The `git remote` command allows you to manage remote repositories: adding or removing them;
* When creating a new remote repository, you link it to Git using: `git remote add origin <url>`;
* To remove a remote repository, use the command: `git remote rm origin`.

## Working with Submodules
* Submodules allow you to include **two or more projects within the same repository**, while keeping their structures separate;
* To add a submodule to your current project, use the command: `git submodule add <repository> <directory>`;
* To check existing submodules, use the command: `git submodule`.

## Updating a Submodule
* Before updating a submodule, **commit any changes** in the submodule;
* To push changes to the submodule repository, use: `git push --recurse-submodule=on-demand`;
* This command will update only the submodule.

### Important considerations
* `git fetch` helps prevent potential conflicts by allowing you to **see updates** before merging them into your local branch.
* Submodules are particularly useful for managing dependencies in large projects, but they require careful tracking and updating to avoid inconsistencies between repositories.
* When using `git pull`, it’s essential to merge changes locally, so you may need to resolve conflicts if your local branch has diverged from the remote.

# Analysis and Inspection

## Displaying Information
* The `git show` command provides various useful details;
* It shows information about the current branch and **its commits**;
* It also displays **file modifications between each commit**;
* You can use `git show <tag>` to display information about tags as well.

## Displaying Differences
* The `git diff <branch>` command is used to show differences between branches;
* When run, it will show the differences between the current branch and the remote branch in the terminal;
* You can also compare specific files by using: `git diff <file a> <file b>`.

## Short Log
* The `git shortlog` command provides a summarized log of the project;
* Each commit will be grouped by the **author’s name**;
* This way, you can easily see which commits were made by whom in the project.

### Important considerations
* `git show` is versatile and can display commit messages, changes, and metadata, which is helpful for inspecting both past and present changes.
* `git diff` is often used before committing to ensure no unintended changes are introduced, especially in collaborative projects.
* `git shortlog` is useful for generating summaries and reports, especially when reviewing contributions for a release or during code reviews.

# Repository Administration

## Cleaning Untracked Files
* The `git clean` command checks and removes files that are not being tracked;
* The `-f` flag can be used to force the cleanup if there are errors;
* This means any files you **haven’t added with git add**;
* It’s used for files that are **automatically generated** and clutter the working directory, making it harder to see what's important.

## Optimizing the Repository
* The `git gc` command is short for **garbage collection**;
* It identifies and removes files that are **no longer needed**;
* This helps optimize the repository in terms of **performance**.

## Checking File Integrity
* The `git fsck` command stands for File System Check;
* It checks the integrity of files and their connectivity;
* This helps detect **possible file corruptions**;
* It’s a **routine command** to ensure everything is correct with your files.

## Reflog
* The `git reflog` command logs all your actions in the repository, even branch switches are recorded;
* On the other hand, `git log` only stores the commits made on a branch;
* **Reflogs are saved until they expire**, with a default expiration of 30 days;
* You can use `git reflog` to retrieve the hash of past changes and navigate back using `git reset --hard <hash>`.

## Archiving the Repository
* With the `git archive` command, you can turn your repository into a compressed file;
* The command is: `git archive --format <type> --output <name.type> <branch>`;
* Example: `git archive --format zip --output main_files.zip main`;
* This will compress the **main** branch into a zip file named **main_files.zip**.

### Important considerations
* Using `git clean` can help remove unnecessary clutter but should be done carefully to avoid deleting important untracked files.
* `git gc` is particularly useful in large repositories where unused objects can slow down performance.
* `git reflog` is a powerful recovery tool, especially when you need to roll back to a previous state after a mistaken change or reset.

# Improving Project Commits

## The Importance of Commits
* **The problem**: meaningless or unclear commits can harm the project’s organization and versioning;
* **Standardizing commits** is essential for the healthy growth of the project, benefiting areas such as:
  * Pull Request **reviews**;
  * Improved clarity in `git log`;
  * Easier project maintenance (e.g., reverting to earlier code versions).

## Branches with Poor Commits
* A solution to this is using **private branches**;
* These are branches **not shared with the repository**, allowing you to commit freely without immediate impact;
* Once the problem is solved, you can use **rebase** to restructure the commits;
* The command would be: `git rebase <current branch> <private branch> -i`;
* During the interactive rebase, you can remove unnecessary commits (`squash`) and rename them (`reword`);
* To save the changes in the terminal, use `:x!` to exit and save the file.

## Writing Good Commit Messages
* Separate the **subject** from the body of the commit message;
* Keep the subject **under 50 characters**;
* Start the subject with a **capital letter**;
* The body should be **under 72 characters** per line;
* Focus on explaining **why** and **what** the commit is doing, rather than describing how the code was written.

### Additional Tips
* Regular use of interactive rebase (`git rebase -i`) helps maintain a clean commit history by merging or editing commit messages.
* Using clear and concise commit messages improves collaboration, especially when multiple developers are working on the same codebase.
* Following a consistent commit message style, such as conventional commits, can greatly simplify tracking changes in large projects.

## Good Commit Message Patterns

Using a consistent pattern for commit messages can significantly enhance the clarity and usefulness of your version history. Here are some commonly used patterns and conventions:

### 1. **Conventional Commits**
The **Conventional Commits** specification provides a structured format for commit messages, helping to automate the release process and changelogs. A typical format looks like this:

```
<type>[optional scope]: <description>
```

**Types of Commits:**
- **feat**: A new feature for the user.
- **fix**: A bug fix for the user.
- **docs**: Documentation changes.
- **style**: Changes that do not affect the meaning of the code (white-space, formatting, etc.).
- **refactor**: A code change that neither fixes a bug nor adds a feature.
- **perf**: A code change that improves performance.
- **test**: Adding missing or correcting existing tests.
- **chore**: Changes to the build process or auxiliary tools and libraries, such as documentation generation.

**Example:**
```
feat(auth): add JWT authentication to the login process
fix(ui): resolve button alignment issue on the dashboard
docs(README): update installation instructions for clarity
```

### 2. **Imperative Mood**
Use the imperative mood in your commit messages, as if you are giving commands. This style makes it clear what the commit does and aligns with how Git generates messages.

**Example:**
- **Good**: `Add user authentication feature`
- **Bad**: `Added user authentication feature`

### 3. **Subject and Body Separation**
Separate the subject line from the body with a blank line. The subject should be a brief summary (50 characters or less), while the body can explain the "why" and "how" of the changes.

**Example:**
```
feat(api): add user profile endpoint

This commit introduces an endpoint for fetching user profiles. 
It supports pagination and includes error handling for invalid user IDs.
```

### 4. **Using a Consistent Prefix**
Using a prefix can help categorize commits at a glance. Common prefixes include:
- **Add**: For adding new features or files.
- **Update**: For modifying existing functionality.
- **Remove**: For deleting features or files.
- **Fix**: For bug fixes.

**Example:**
```
Add: new payment processing feature
Update: modify user settings page
Remove: deprecated API endpoints
```

### 5. **Be Descriptive**
Ensure that commit messages are descriptive enough to provide context. Avoid vague messages like "fix stuff" or "change things." Explain what was changed and why.

**Example:**
```
fix(auth): correct token expiration logic to prevent premature logouts
```

### 6. **Avoid Emoji Overuse**
While some teams use emojis to add a visual element to commit messages, it’s generally best to limit their use to avoid confusion and maintain professionalism.

**Example:**
```
feat: 🎉 introduce new notification system
```

### 7. **Use Ticket References**
If you’re using a project management tool like Jira or GitHub issues, reference ticket numbers in your commit messages for easy tracking.

**Example:**
```
fix: resolve issue with user login error (JIRA-123)
```

### Conclusion
Following a structured and clear commit message pattern enhances collaboration and makes it easier for teams to understand the project's history. By adopting these patterns, you can ensure that your commits are meaningful and contribute positively to the overall codebase.