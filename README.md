# Git Submodule Check

## Introduction

When working with git [submodules](https://www.kernel.org/pub/software/scm/git/docs/git-submodule.html) the worst part is dealing with _missed_ commits.
That usually happens when you make changes to the submodule, but forget to push it to its remote repository __before__ merging/pushing the _parent_ repository.

There are many posts regarding this
- http://codingkilledthecat.wordpress.com/2012/04/28/why-your-company-shouldnt-use-git-submodules/
- http://stackoverflow.com/questions/12075809/git-submodules-workflow-issues
- http://allgood38.io/how-to-break-your-git-submodules.html

## pre-commit hook

The goal of this script is to help avoid this issue by comparing the local commits for each submodule against their corresponding commits on the remotes. If there's a mismatch, then the commit is aborted.

### Installation

Copy this `pre-commit` script to `.git/hooks` folder on the repository you want to monitor.
Now, everytime you try to commit the _parent_ repository, the script will run and check the commit SHAs for each submodule to make sure you have the latest copy and you did not miss any push.

### Sample Outputs

If all goes well, this is what you should see and commit will succeed:

```bash
user@mac ~/Work/MyClonedRepo (master)$ git commit -am "pushing parent and submodule is good" 
Checking submodules...
  Submodule [Vendor/lib_1] ... OK
  Submodule [Vendor/lib_2] ... OK
  Submodule [Vendor/lib_3] ... OK
returning state 0
[master 73ba08d] pushing parent and submodule is good
 1 file changed, 1 insertion(+)
user@mac ~/Work/MyClonedRepo (master)$ 
```

Otherwise, an error message is displayed and it lists the SHAs to help you resolve the issues:
```bash
user@mac ~/Work/MyClonedRepo (master)$ git commit -am "pushing parent and submodule is wrong"
Checking submodules...
  Submodule [Library/SampleLib] ... FAILED
 
Stop! Pre-commit condition failed.
Did you forget to push submodule [Library/SampleLib] to remote?
Cannot proceed until you do so.
Remote HASH: 12f8c095710278aadf18f04e3ca026ac7cdd7e72
Local HASH.: 3281d051f138bcd8c48d431ecdbdc54c2c6987db

user@mac ~/Work/MyClonedRepo (master)$ 
```

## Notes
- Script was tested with Mavericks 10.9.2
- submodules of submodules are not checked

