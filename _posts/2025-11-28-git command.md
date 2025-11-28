---
layout: post
title: "How to use git.bash command?"
date: 2025-11-28 12:00:00 +0900
author: kang
categories: [github, git.bash]
tags: [github, git.bash, command]
pin: false
math: true
mermaid: true
---

# How to use git.bash command

* CHECK <span style="color:brown">PATH</span> and <span style="color:brown">STATUS</span>
  -
  ``` C++
  # 'ls' = list of files in current directory
  # 'ls' -a = show with hidden files
  # 'ls' -l = show with detail of files
  # 'ls' -r = get reverse about sorting of files
  # 'ls -t' = show list of files on decrease
  # 'ls -la' = 'ls -a' + 'ls -l'
  ```

  ``` C++
  # ' - '  = current directory is home directory
  # 'pwd' = show current directory
  # 'cd ..' = go to parent directory
  # 'cd User' = go to subdirectory, the name is 'User'
  # 'cd ~' = go to home directory
  # 'cd c:' = go to 'c directory'
  ```

* REGIST <span style="color:brown">GIT.INFO</span>
  -
  ``` C++ 
  # 'git config --global user.name "WORK"' = assign the user name, in this case name is WORK
  # 'git config --global user.email "WORK@gmail.com"' = assign the user.email, in this case user email is 'WORK@gmail.com'
  # 'git config --global --list' = show current global user info
  ```

* COMMAND <span style="color:brown">GIT.BASH</span>
  - 
  * INIT, STAGING and COMMIT
  ``` C++ Re
  # 'git init' = make git initalization files
  # 'git version' = current git version
  # 'git status' = show current git status
  # 'git add hello.txt' = staging hello.txt file
  # 'git add .' = staging all of modfied files
  # 'git commit -m "message you want"' = add the message 'message you want' when we commit the files
  # 'git commit -am "message twice"' = staging and commit at once with message that is 'message twice'
  # 'git commit --amend' = change lastest commit message
  ```

  * COMPARE BEFORE and AFTER
  ``` C++
  # 'git diff' = what is difference of lastest commit and now
  # 'git diff HEAD origin/main' = show what is difference between HEAD(local) and origin/main(remote)
  ```

  * GET BACK to the before
  ``` C++
  # 'git restore .' = remove the modified files right after commit
  # 'git restore --staged .' = cancel the staging
  # 'git restore hello.txt' = get hello.txt back to before unmodified
  # 'git reset HEAD^' = cancel commit and staging
  # 'git reset --soft HEAD^' = cancel commit and make staged
  # 'git reset --mixed HEAD^' = cancel commit and make unstaged == 'git reset HEAD^'
  # 'git reset --hard "commit hash value"' = move to 'commit hash value' and the front of commits are removed and lastest commit is 'commit hash value' commit
  # 'git revert "commit hash value"' = get revert commit to commit hash value time
  ```

  * BRANCH
  ``` C++
  # 'git branch' = confirm branch status
  # 'git branch "name"' = make branch named name
  # 'git switch "branch.name"' = switch the branch X to branch.name
  # 'git merge A' = Merge branch A to current branch
  # 'git branch -d A' = remove branch A
  ```

  * MANANGEMENT
  ``` C++
  # 'git log' = show commit history until now`
  # 'git log --oneline' = show commit history until now using brief oneline
  # 'git log --oneline --branches' = show the branches status
  # 'git log --oneline --branches --graph' = show the branches status with graph
  # 'git log --oneline --all --graph' = show not only the branches but also stash, remote master, tags status with graph
  # 'git log A..B' = show the commit B that A branch did not have
  # 'git cherry-pick "commit hash value of other branch"' = cherry pick a specific commit
  ```
  * MANAGEMENT WITH REOMOTE
  ``` C++
  # 'git remote add origin "HTTPS address"' = connect remote origin
  # 'git remote -v' = show remote status
  # 'git push -u origin main' = push remote repository from local at the just first time
  # 'git push' = push remote repository from local commit
  # 'git pull origin main' = request to pull the lastet commit from remote repository
  # 'git fetch' = get the remote information without merge : fetch + merge = pull
  ```

  * [ADVANCED] SSH
  ``` C++
  # 'ssh-keygen -t ed25519 -C "user@email.com"' = generate ssh key
  # 'eval "$(ssh-agent -s)"' = automatically verify ssh system on environment valuable
  # 'ssh-add ~/.ssh/id_ed25519' = regist id_ed25519 files in RAM for verification
  # 'clip < ~/.shh/id_ed25519' = copy the info of id_ed25519 to the clipboard
  ```

  * [Extra] functions
  ``` C++
  # 'clear' = clear the screen of git bash
  # 'mkdir test' = make new folder, the name is 'test' folder
  # 're -r test' = remove folder of name that is test. even it will be delete all of files in 'test' folder
  # 'exit' = exit the git bash
  # 'touch A' = make empty file A
  ```

  * [Extra, not command] 
  ```C++
  # '.gitignore' file = manage files when we adjust version
  ```








