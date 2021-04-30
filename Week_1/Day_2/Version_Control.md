### Version Control

**Git** - What, why git?

* Repositories (one repo per project)
* Save milestones
* Keeps a history of your code (commits)
* Backup copy on GitHub
* Work better as teams, branches
* Do use git
* You **will have to use** git in team projects

**Notes on git**

* Using `ls -al ` will show us the .git folder, letting us know what's initialized with git
* `git add` will add the files to the staging area
  * red means it's not added yet; green means it's been added
* `git commit -m "..."` needs to have a meaningful message
* `git log` will show the history of the commits, the author, the date.
* `git branch` to learn which branch we're on
* `git remote -v` shows which remote we're connected to
* `git remote add origin <URL>` will connect the folder/files to the repo on GitHub
* `git remote rm origin` will remove the folder/files from the repo on GitHub
* `git branch` will indicate which branch you're on
  * `git branch -M <NAME>` will allow you to change the name of the branch (for example, to `main`).
  * Common practice is to have the main branch be called `main` and **not** `master` anymore.
* `git push -u origin main` will set the upstream to the main branch 
  * `-u` allows it to be connected to the main branch upstream, so you can use just `git push` in the future (without `origin main`).
* `git checkout` to change the branch
  * `git checkout -b <NAME>` allows you to create a new branch the first time

(if you're using HTTPS for the repo, you will be asked to authenticate each time with each push or pull; SSH is easier to use as it authenticates automatically)
