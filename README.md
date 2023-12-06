# Learn How to Use Git

## Dec. 6th 2023 Update

### How to use `git-lfs` to upload large files?

DO REMEMBER: `git-lfs` is not completely free in github

setting -- Billing and plans -- Plans and usage

![](https://gzhlibrary.oss-cn-beijing.aliyuncs.com/img/202312062214926.png)

Install the `git-lfs` firstly:

```bash
apt-get install git-lfs # In Linux
brew install git-lfs # In macos
```

Set up the lfs using for current git account.

**You only need to run this once per user account.**

```bash
git lfs install
```

Set up which file to track using `git-lfs`:

```bash
git lfs track "*.zip"
```

And a file named `.gitattributes` would be generated.

```bash
git add .gitattributes
```

Check the files tracked by `git-lfs` currently:

```bash
git lfs ls-files
```

And then use `git add .` , `git commit -m "comment"` and `git push` to push to the remote repo.

## Original

## Install Git on Linux (WSL)

I need to config the environment to tell the Git where to commit or push the data.

```shell
$ git config --global user.name ZhongJunhong
$ git config --global user.email [My GitHub login in email]
```

The global configuration can be changed in the future in such hidden document: `~/.gitconfig`:

```shell
$ cd # change to user home dir
~$ vim .gitconfig
```

## Initialize the Git repo

Create a folder and change dir to it.

```shell
$ mkdir learngit
$ cd learngit
```

Initialize such dir as Git repo.

```shell
$ git init
```

This operation will add a hidden file named ".git" to local dir.

```shell
$ ls -a
. .. .git learngit.md
```

## Git add & commit

To understand `git add` and `git commit` command, it' s necessary to specify the Git workflow:

Initialize the Git repo, such repo (the work folder actually) is called `master` branch in Git.

When we change something in repo, the change will not be added to `master` directly, they should be moved to a place that we called `stage` in Git **for temporary**.

Add a changed file to `stage`.

```shell
$ git add [filename]
```

Or add all changed files to `stage`.

```shell
$ git add .
```

Step3: All change added to `stage` need to be moved to `master` branch.

```shell
$ git commit -m "first commit"
```

**Commit comment could not be skip. It 's necessary to specify what have been changed in every commit time.**

Every commit is to specify a version of such repo. It 's the key for Git to achieve **version control**. 

And we will see the respon from Git:

```shell
1 file changed, 2 insertions(+), 1 deletion(-)
```

## Check Git status

To check Git status by command:

Respond showed below mean something has been changed but not commit.

```shell
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   learngit.md

no changes added to commit (use "git add" and/or "git commit -a")
```

After `git add`:

```shell
$ git add .
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   learngit.md
```

After `git commit`:

```shell
$ git commit -m "test"
[master 78da7cf] test
 1 file changed, 9 insertions(+)
$ git status
On branch master
nothing to commit, working tree clean
```

## Connect to GitHub

Create local personal public and privace ssh key.

```shell
$ cd # Change to user home dir.
~$ ssh-keygen -t rsa -C [GitHub login email address] # Generate personal public ssh key and private ssh key.
$ cd .ssh
$ ls
id_rsa id_rsa.pub # id_rsa stored the private ssh key that can not be share. The other is the public ssh key.
$ vim id_rsa.pub # open the public ssh key, and copy all content in it.
```

Add ssh key to GitHub account. Just turn on the personal account setting, and direct to ssh setting, create a new ssh key and name it what you like. Here 's no detail cause I need to protect my privacy.

**ssh key is used for GitHub to identify the permission of who commits to the repo.**

*More detail about differencies between privace and public ssh key please search on Google, this is really a good way instead of identification by username and password.*

Create a GitHub repo. Here I create a repo named learngit for example.

I have told that the local repo called `master` in Git above, and the GitHub remote repo we create here called `origin` in Git, but it named `master` still on GitHub you can see.

Set up the remote `origin` for the local repo.

```shell
$ git remote add origin git@github.com:ZhongJunhong/learngit.git
```

Connect to GitHub repo and push.

```shell
$ git push -u origin master
Enumerating objects: 57, done.
Counting objects: 100% (57/57), done.
Delta compression using up to 4 threads
Compressing objects: 100% (38/38), done.
Writing objects: 100% (57/57), 5.85 KiB | 498.00 KiB/s, done.
Total 57 (delta 18), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (18/18), done.
remote: 
remote: Create a pull request for 'master' on GitHub by visiting:
remote:      https://github.com/ZhongJunhong/learngit/pull/new/master
remote: 
To github.com:ZhongJunhong/learngit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

In the first time pushing to remote repo, I specify parameter `-u` to **set the connection between remote `origin` branch and local `master` branch.**

And the parameter `-u` can be skipped in the future like:

```shell
$ git push origin master
```

Or just:

```shell
$ git push
```

## Keeping update to the remote resp

```shell
$ git pull
```

## Some Detail

### Pay attention to the correct `origin` set up

It 's necessary to enter the correct command expecially the username when we connect to GitHub repo cause when the `origin` be set up once it can not be changed later until remove it.

Remove the origin of the local repo.

```shell
$ git remote remove origin
```

Then repeat the operation above.

### Difference between `master` and `main` branch on GitHub

From several years ago, GitHub began to use the `main` for default branch name instead of `master` that used before.

For now we can easily move content from a branch to another. Here I assume that you have set up `master` branch locally and remotly.

Before beginning, it 's necessary to explain what is `branch` in Git:

A `branch` is likely a "sandbox" workspace in a project. Whatever you have done inside a branch, there would be nothing can affect other branch until someone have permission merge two branchs.

#### Case1 change the global defalut branch name before `git init`

```shell
$ git config --global init.defaultBranch main
```

#### Case2 remote repo already have a default branch named `main` at initialize time.

First, build a branch named `main` locally.

```shell
$ git checkout -b main
```

We can check all branch and the current branch locally.

*The star \* ahead of one of the branch name means the current branch.*

```shell
$ git branch
  master
* main
```

Merge the local `master` branch to local `main` branch. **Do remember to `add` and `commit` in `master` branch before merging.**

```shell
$ git merge master
Updating 50323a6..92033e8
Fast-forward
 learngit.md => README.md | 50 +++++++++++++++++++++++++++++++++++++++++++++-----
 1 file changed, 45 insertions(+), 5 deletions(-)
```

And then do remember to  `add` and `commit` in `main` branch.

```shell
$ git add .
$ git commit -m "merge from master branch"
```

Then push it to remote branch.

```shell
$ git push -u origin main # for initialized time, can be skipped in the future.
```

The local and remote `master` branch can be deleted now. But you 'd better check the pushing are successful on GitHub before your deleting.

Delete the local `master` branch.

```shell
$ git branch -D master
```

Delete the remote `master` branch.

```shell
$ git push origin -D master # D means force deleting
```

And I have meet some error before:

```shell
int: Updates were rejected because the tip of your current branch is behind its remote counterpart. Integrate the remote changes (e.g. hint: 'git pull ...') before pushing again.
See the 'Note about fast-forwards' in 'git push --help' for details.
```

Such error occured cause I have made some change on remote branch by GitHub web interface that have not merge locally. And I finally `rebase` the branch between remote and local branch to fix it.

```shell
$ git pull --rebase origin main 
```