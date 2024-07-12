--- 
date: 2020-04-13
slug: git-large-files
title: Locating and Removing Large Files from Git History

authors:
  - kono94 
categories:
  - Programming
  - Tooling
tags:
  - Git
  - Gitlab
  - Linux
  - Mac
---

This article is about checking your Git history for large files and removing them if they got pushed accidentially because Git histories should not include large binaries, artifacts, videos or alike. There are other tools like DVC or git-lfs for versioning big files or datasets.
<!-- more -->
## Check your repo for large files

### On Linux
```
git rev-list --objects --all \
 | git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' \
 | sed -n 's/^blob //p' \
 | sort --numeric-sort --key=2 \
 | gcut -c 1-12,41- \
| $(command -v gnumfmt || echo gnumfmt) --field=2 --to=iec-i --suffix=B --padding=7 --round=nearest
```

### On Mac
```
brew install coreutils
```

For Mac one has to adapt the command by changing ```cut``` to ```gcut```
and ```numfmt``` to ```gnumfmt```

```
git rev-list --objects --all \
 | git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' \
 | sed -n 's/^blob //p' \
 | sort --numeric-sort --key=2 \
 | gcut -c 1-12,41- \
| $(command -v gnumfmt || echo gnumfmt) --field=2 --to=iec-i --suffix=B --padding=7 --round=nearest
```


### Add as alias
Add this to your ```~/.bash_profile``` or ```~/.bashrc``` :
```
if [ -f ~/.bash_aliases ]; then
. ~/.bash_aliases
fi
```

Create or add to your ```~/.bash_aliases```:
```
  alias git_large="git rev-list --objects --all | git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' | sed -n 's/^blob //p' | sort --numeric-sort --key=2 | gcut -c 1-12,41-  | $(command -v gnumfmt || echo gnumfmt) --field=2 --to=iec-i --suffix=B --padding=7 --round=nearest"
```

<br><br>
### Remove big files and force push

#### Option 1 (not preferred)
<br>

   ```
git filter-branch --index-filter 'git rm --cached --ignore-unmatch file1 file2' HEAD
```

```
git reflog expire --expire=now --all && git gc --prune=now --aggressive
```

   ```
git push --force
```

<br>
#### Option 2

Get BFG-repo-cleaner:

```
git clone https://github.com/rtyley/bfg-repo-cleaner.git
```

or 

```
brew install bfg
```

Cleanup unnecessary files and optimize the local repository:

```
git gc
```

Delete all files from history which are bigger than 10 Megabytes:

```
dfg --strip-blobs-bigger-than 10M .git
```

or

```
java -jar ~/opt/bfg-1.13.0.jar --strip-blobs-bigger-than 10M .git
```

Reflect your changes:
```
git reflog expire --expire=now --all && git gc --prune=now --aggressive
```

Push to remote ("with-lease" => this will fail if someone updated the HEAD of the master branch):

```
git push origin master --force-with-lease
```

In Gitlab go into the `Settings > Repository > Repository cleanup` and upload the
`object-id-map.old-new.txt` file, located in the `.git.bfg-report`-folder

__Done!__