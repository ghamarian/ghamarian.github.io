
---
layout: post
title:  "Git useful commands"
date:   2020-05-07 11:47:03 +0100
categories: Git
---

1. `git stash push --keep-index` makes the working area look like the staged area and stashes the working area changes
2. `git stash pop` now removes the stashes
3. `git branch --no-merged` shows the branches that are not merged
4. `git branch --contains <branch-name>`
5. `git branch -m <old-branch-name> <new-branch-name>`
6. `git tag -l <pattern>` such that `git tag -l *v0.6*`
7. `git ls-remote --tags origin` to get the list of tags in origin
8. `git checkout :/"message"` checkout to a commit that has a message
9. `git log -1 --summary --find-renames` includes whatchanged and detectes renames
10. `git log --merge --left-right` only shows commits that are influencing the current merge conflict
11. `git checkout <file> --theirs(--ours)` during the conflict, accept theirs(ours)
12. `git log -S"<word that was added your removed>`
13. `git branch --contains <commit>` which branches contain a particular commit.
14. `git rev-parser :<filepath>` returns the hash of the staged file


----
### Plumbing 
1. `git hash-object <filename>` shows the hash
2. `git cat-file -p <hash>` shows the content of a commit
3. `git ls-tree <commit>` shows the content of a tree like commits


[Nvidia-forum-instruction]: https://forums.developer.nvidia.com/t/pytorch-for-jetson-nano-version-1-5-0-now-available/72048
[Opencv-installation]: https://www.jetsonhacks.com/2019/11/22/opencv-4-cuda-on-jetson-nano/
[opencv-script]: https://github.com/AastaNV/JEP/blob/master/script/install_opencv4.1.1_Jetson.sh
