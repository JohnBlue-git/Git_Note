## Git command

### Configuration
ref: https://dev.to/mrtrom/git-config-a-beginners-guide-with-advanced-tips-393f#:~:text=Open%20your%20terminal%20and%20type:%20git
```console
# Basic configuration
# (remove the --global parameter if you only want to do this for a specific repository)
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
git config --global user.username "yourusername"

# Signing configuration
git config --global gpg.format ssh
git config --global user.signingKey ~/.ssh/id_rsa.pub

# Check your Git configuration
git config --list

# Other configuration

# To add --rebase as default of git pull
# (in ~/.git/config or ~/.gitconfig)
[branch "master"]
  remote = origin
  merge = refs/heads/master
  rebase = true
# or
git config --global --bool pull.rebase true

# To configurue merge (in ~/.git/config or ~/.gitconfig)
[merge]
    ff = no
    commit = no
# or
git config --global merge.commit no
git config --global merge.ff no
```

### .gitignore
The purpose of a .gitignore file is to specify which files and directories Git should ignore in a project. This helps keep your repository clean and free from unnecessary files that do not need to be tracked.
\
\
Good place to find sample .gitignore: https://github.com/github/gitignore

### Starting a Repository
```console
# Create an empty Git repository or reinitialize an existing one.
git init

### Staging change
```console
git add <file / folder>
git add .
```
### Un-staging change
```console
# unstage all (softly) 
git reset HEAD

# unstage specific
git rm <-rf> --cached <file/ folder>
# unstage specific (safer way)
git checkout HEAD -- <file/ folder>
```
### Show the status of working directory and staging area
```console
git status
```
### Commit changes to the repository
```console
git commit -m "Commit message"
```
### Inspecting & Comparing
```console
# show log
git log
# last <count>
git log -p -<count>
# Shows commits after a specified time
git log -since=<time>
# Shows commits before a specified time
git log -until=<time>
# prettify
git log --online

# get current <commit hash>
git rev-parse HEAD
# to get shorter hash:
git rev-parse --short HEAD

# show diff compared with stage changes
git diff
# show diff between two commits
git diff <commit1> <commit2>
# create .patch (result from stage changes)
git add .
git diff --cached > <patch file>

# show diff (contains commit information)
git format-patch <branch1> <branch2>
# create .patch (--stdout for single file)
git format-patch <branch1> <branch2> --stdout > <patch file>
# Create patches for the last 3 commits
git format-patch -3
# Create a patch for a specific commit
git format-patch -1 <commit hash>

# Check patch
git apply --stat <patch file>
git apply --check <patch file>
# Apply the patch
git apply <patch file>
# Apply the patch with commit history
git am <patch file>

# Git blame (a useful tool for investigating the history of changes to a file.)
# Basic Usage
git blame <filename>
# The output of git blame looks something like this:
# commit_hash (AuthorName YYYY-MM-DD HH:MM:SS +timezone) LineNumber) Content of the line

# -L: Limit the blame to specific line ranges.
git blame -L 10,20 <filename>

# -e: Show the email addresses of the authors instead of the usernames.
git blame -e <filename>

# -C: Detect moved or copied lines within the repository.
git blame -C <filename>

# -M: Detect moved or copied lines within the same file.
git blame -M <filename>
```
### Branching & Merging
```console
# List branches
git branch -a
git branch -av
# Create branch
git branch <branchname>
# Delete branch
git branch -d <branchname>
git branch -D <branchname>

# Switch commit
git checkout <commit>
# Switch branch
git checkout <branchname>
# Create branch
git checkout -b <newbranch>
# Checkout and Create branch
git checkout -t <origin/kirkstone> -b <my-kirkstone>
# Clone remote branch
git checkout -b <local_branch_name> origin/<branch_name>
# !!! if "git branch -av" show remotes/insydegerrit/RC24.12
git checkout -b  RC24.12 remotes/insydegerrit/RC24.12

# To merge <feature-branch> into <main>
git checkout <main>
git merge <feature-branch>

# If you prefer to keep the feature branch history but still want to merge it into the main branch
git merge --no-ff <feature-branch>
```
### Remote Repositories
```console
# What does `origin` mean in Git?
# `origin` is just a **default name** Git uses for the **remote repository** you cloned from.
# You can inspect your remotes with:
git remote -v
# origin  https://android.googlesource.com/platform/frameworks/base (fetch)
# origin  https://android.googlesource.com/platform/frameworks/base (push)
# * The remote is called `origin`
# * Its URL is `https://...`
# * It's used for both fetching and pushing

# add a remote repository
git remote add <new branch name> <url>

# Rename a remote
git remote rename <old> <new>

# Change remote URL
git remote set-url <name> <url>

# list remotes
git branch -r

# Download objects and refs from another repository
# Does not merge anything into your current branch
# Does not switch your branch or modify files
git fetch <remote>

# Fetch from and integrate with another repository or a local branch.
git pull <remote> <branch>
# special note:
# 如果你有未提交的更改，则 git pull 命令的合并部分将失败，而你的本地分支将保持不变。
# 有 conflict 怎麼辦? 出現 conflict 一會暫停動作，需要你手動修復後，然後才可以繼續動作。
# 要整齊一點的紀錄，使用git pull --rebase
# 預期會有較多的 conflict，建議用 merge 或多開 branch; 如果修改範圍較小，不太預期有 conflict，則建議可以加上 rebase 參數。

# Clone
git clone -b <branch> <url> <output folder name>

# Update remote
git push <remote> <branch>
git push origin main
# Update from feature branch
# This command will create a review for your changes, and once the review is approved, it will be merged into the master branch by someone with the necessary permissions.
git push origin <local feature branch>:refs/for/master
# !!! if "git branch -av" show remotes/m/master
git push insydegerrit  <local feature branch>:refs/for/master
```
### git cherry-pick
```console
# usage:
#   Actions allow us to select one or more specific commits from one branch and apply them to another
# comparsion:
#   cherry-pick: Useful for selecting specific commits individually and applying them to other branches.
#   rebase: Good for merging branches, keeping commit history clean and intuitive.
git cherry-pick <commit-hash from another branch>
```

### Stash changes to a dirty working directory
```console
# stash
git stash 

# stash apply
git stash apply
```
### Reset / Revert
```console
# Moves the HEAD to <commit>.
# Keeps changes in the working directory.
# Changes remain staged. 
git reset --soft <commit>:
git reset --soft HEAD~1

# (default behavior)
# Moves the HEAD to <commit>.
# Resets the index to match <commit>.
# Changes are kept in the working directory but not staged.
git reset --mixed <commit>
git reset --mixed HEAD~1

# Moves the HEAD to <commit>.
# Resets the index and working directory to match <commit>.
# Discards all changes and staged files. 
git reset --hard <commit>
git reset --hard HEAD~1
# If you have run git reset --hard HEAD and still see uncommitted changes with git status, it likely means there are changes in your working directory that are being tracked by Git but are not affected by the reset command.
# Use git clean -f to remove untracked files
git clean -f
# Check for ignored files: Sometimes, .gitignore files can cause files to be ignored by Git. These files will not be affected by git reset --hard. This command will remove all untracked and ignored files, so use it with caution.
git clean -fdx

# Unstages <file>, but preserves its contents. git reset HEAD index.html
git reset HEAD <file>:

# Creates a new commit that undoes the changes made by <commit>.
git revert <commit>:
```
### HEAD
```console
# refers to the state of HEAD two moves ago in the reflog.
HEAD@{2}
# refers to the commit that is two parents before the current HEAD.
HEAD~2
```

### Rebase
To move or combine a sequence of commits to a new base commit. It's a powerful tool for altering commit history in a linear sequence.
```console
# command
git rebase -i HEAD~5
# --interactive or -i: The -i flag stands for "interactive". When you use this flag, Git opens an interactive editor where you can control how each commit will be handled during the rebase process.
# HEAD~5: This part specifies the range of commits to be included in the rebase. HEAD represents the latest commit on the current branch, and ~5 indicates the last 5 commits before HEAD.
```

After command git rebase, Git will open the editor. In the interactive editor, you can choose actions for each commit such as:
- pick: Use the commit as is (default action).
- reword: Use the commit, but edit the commit message.
- edit: Use the commit, but stop for amending.
- squash: Combine this commit with the previous one.
- fixup: Combine this commit with the previous one, but discard the commit message.
- drop: Remove the commit entirely.

Here's an example of what the interactive editor might look like:
```console
pick 123abc Commit message 1
drop 456def Commit message 2
```

### Archive
```console
# git archive 是一个用于创建 Git 仓库中某个提交（commit）或分支（branch）的归档文件的命令。它生成一个包含指定内容的归档文件，比如 ZIP 或 TAR 文件。这个功能很有用，例如当你需要将某个特定版本的代码打包分发给其他人时。
git archive --format=zip --output=archive.zip HEAD
git archive --format=tar --output=archive.tar HEAD
git archive --format=zip --output=archive.zip <commit-id>
git archive --format=zip --output=archive.zip HEAD path/to/directory
```
### supplement: how to set git clone with ssh
https://tsengbatty.medium.com/git-%E8%B8%A9%E5%9D%91%E7%B4%80%E9%8C%84-%E4%BA%8C-git-clone-with-ssh-keys-%E6%88%96-https-%E8%A8%AD%E5%AE%9A%E6%AD%A5%E9%A9%9F-bdb721bd7cf2
### supplement: git push 
https://docs.github.com/en/get-started/using-git/pushing-commits-to-a-remote-repository

### Commit common regulation
message structure
```console
<type>(<scope>): <subject>
<body>
<footer>
```
type:
- common:
  - feat: New features, new features
  - fix: Fix the bug
  - perf: Changes to the code to improve performance (optimization of program performance without affecting the internal behavior of the code)
  - refactor: code refactoring (refactoring, code modification without affecting the internal behavior and functionality of the code)
  - docs: Document modifications
  - style: code formatting modifications, note that they are not css modifications (e.g. semicolon modifications)
- others
  - test: A test case is added or modified
  - build: Affects project builds or dependency modifications
  - revert: Reverts the last commit
  - ci: Continuous integration related file modifications
  - chore: Other modifications (modifications that are not of the above type)
  - release: Releases a new version

scope:
- commits affect the range, e.g. route, component, utils, build...

subject: 
- commit subjective

body:
- commit specific modifications, which can be divided into multiple lines.

footer:
- some notes, usually a link to a breaking change or a bug fixed.

### Benefit to use .patch
- Modularity and Isolation:
  - **`.patch`** files allow you to isolate specific changes or fixes. Each patch corresponds to a specific issue or feature, making it easier to manage and track.
  - When you apply a patch, it modifies only the relevant part of the codebase, keeping other parts untouched. This modularity helps prevent unintended side effects.
- Collaboration and Code Review:
  - When you commit directly to the source code, it affects the entire repository. This can be problematic when multiple developers work on different features simultaneously.
  - **`.patch`** files facilitate collaboration by allowing developers to share their changes without committing them directly. Others can review the patches, provide feedback, and suggest improvements before merging them.
- Upstream Compatibility:
  - When you maintain a fork of an open-source project, using patches allows you to stay in sync with upstream changes.
  - Instead of modifying the original source directly, you apply patches on top of it. This way, you can easily update your fork with the latest changes from the upstream repository.
- Portability and Distribution:
  - **`.patch`** files are portable across different systems and environments.
  - Distributing patches alongside the source code allows others to apply them to their own copies of the codebase, ensuring consistency.
