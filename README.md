Git Submodule Check
===================

Introduction
------------

When working with git [submodules](https://www.kernel.org/pub/software/scm/git/docs/git-submodule.html) the worst part is dealing with _missed_ commits.
That usually happens when you make changes to the submodule, but forget to push it to its remote repository __before__ merging/pushing the _parent_ repository.

There are many posts regarding this
- http://codingkilledthecat.wordpress.com/2012/04/28/why-your-company-shouldnt-use-git-submodules/
- http://stackoverflow.com/questions/12075809/git-submodules-workflow-issues
- http://allgood38.io/how-to-break-your-git-submodules.html

pre-commit hook
---------------

The goal of this script is to help avoid this issue from happening.
You add it to your `.git/hooks` __local__ folder and it will run everytime you try to commit changes to the _parent_ repository.
For each submodule, the script verifies whether the remote repository is synced with the latest commits on your local clone. If there's a mismatch, then you might be missing something:
- either you don't the latest commits from the remote submodule
- you forgot to push your local changes to the submodule repository

In either case, the script will abort the commit and avoid all the mess.

