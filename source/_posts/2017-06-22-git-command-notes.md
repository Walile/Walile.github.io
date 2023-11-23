---
layout: post
title: Git指令速記
date: 2017-06-22 22:34:19
tags: frontend
---

Git指令速記 (因為太常忘記了)

<!--more-->

### git branch 
1. branch: git branch <code>git branch <branch_name></code>
2. delete a branch <code>git push origin --delete <branch_name></code>
3. local branch to remote: <code>git push -u origin {branch_name}</code>

### tag
1. list tag: <code>git tag</code>

2. 打tag: 
3. push all tag到remote: <code>git push origin --tags</code>
4. checkout remote tag: <code>git checkout tags/version {version number}</code>


### git checkout
1. checkout remote branch (new version git): 
<code> git fetch</code>
<code> git checkout <branch_name></code>

2. 

### git revert

<code>// revert code to specific point you want to keep
git reset --hard {hash}</code>

<code>// move branch pointer back to previous HEAD
git reset --soft HEAD@{1}</code>

<code>// remember commit revert change
git commit -m "revert to {hash}"
</code>

### git rebase