.. _contributing/how-to-contribute:

===================================
Getting Help and Ways to Contribute
===================================



Setup environment for Alignak :
===============================

Alignak require python-pbr to be setup (and so python). We suggest to use virtualenv also ::

  virtualenv alignak_env
  . alignak_env/bin/activate
  python setup.py develop

if you don't want virtualenv (you are in a docker or something), just add sudo / install it on you system.


Getting started into the developer documentation :
==================================================

The `developer documentation`_ is generated from Alignak source code and basically describe its modules.
You can see in details the functions and jump to the source code if necessary. You also have some class diagram in the index page to browse code more easily
A good entry point could be the daemon module, you can find the python files used to launch daemons (arbiter, scheduler ...).



Git and GitHub 101 :
====================

Before starting to dig into Alignak code, you should be able to use git with ease. If you are new to it, we can suggest you the following links : http://www.git-tower.com/blog/git-cheat-sheet/ and http://www.cheat-sheets.org/saved-copy/git-cheat-sheet.pdf

If you are already familiar with this, here are some of useful commands we use quite often.
Consider "origin" the remote branch from Alignak-monitoring organization

Add the fork you have made by pressing the "Fork" button on Github ::

  git remote add <yournick> git@github.com:<yournick>/alignak.git   # Add your remote git
  git fetch <yournick>   # Fetch data from this remote
  git checkout <yournick>/develop -b mydevelop  # Create new branch named mydevelop linked to the remote develop branch of you fork


Synchronize alignak develop with your current branch (considering no conflicts)::

  git fetch origin
  git merge origin/develop  # You should have a merge commit to confirm
  git log  # Pick the id of the commit before the merging commit you have just done
  git rebase <commit-id>  # git will automatically try to stack commit over the commit you specified (develop HEAD)
  git push -f <yournick> <current_branch>  # This push the new tree upstream (we have to force push as your local and remote have drifted)


Avoid merge commit because you forgot to pull before committing::

  git pull  # This will fetch and merge remote branch with your local one creating a merge commit
  git log   # Pick the id of the commit corresponding to the remote HEAD (usually 2 commit before)
  git rebase  <commit-id>  # Git will revert you commit(s) and stack it after the remote HEAD
  git push  # Don't need to force you have only added one commit over remote.


Clean a Pull request before submitting it::

  git rebase -i  <commit-id>  # Pick the id of the current origin develop (see synchronize)
  # Here you  squash, fix, reword or edit order of commit the way you want.
  # At the end git will try to make the tree the way you ask (if no conflict)
  # I case of conflict git will stop were the conflict is and let you deal with it
  # git mergtools may help for that
  # One you are done (git status says there no modified file neither added file)
  # git rebase --continue


Step by step contribution example :
===================================

The process to contribute to Alignak is quite simple. However, depending on what you are planning to do, the most efficient way to do it may vary.

Simple fix :
------------
If you have a very small to do (typo, doc, one file commit), you'd better use Github. You can click edit in the Alignak repository.
It will fork the repository for you and let you edit the file through the Web Interface.
Once you picked a good commit message (see below for commit message habits) you can push it in a new branch (see below for branch name habits).
Finally, you can create a new pull request to the Alignak repos (still with Github UI)


Not so simple fix :
-------------------

Checkout new branch :
~~~~~~~~~~~~~~~~~~~~~
If you have more than that then, no magic, you will have to do the whole process "manually"
Once you have forked the repository and added remote (see above) you can start a new branch ::

  git checkout -b Add_ponies_and_rainbows

Here your new branch is Add_ponies_and_rainbows. You can now start editing files

Run tests :
~~~~~~~~~~~
Before making commit (unless you know that you are pushing unfinished stuff) you should run tests.
If you have enabled Travis on your fork (recommended) and does not run tests you may received a mail from it noticing that your commit broke tests.
To run the test cases do the following ::

  cd test
  ./quick_tests.sh

This script launches all test_*.py file and perform a pep8 check. This more or less the same thing that Travis does.
If you want to run the same thing that Travis does, have a look at .travis.yml in the root tree.
You will find something like ::

  nosetests -xv --process-restartworker --processes=1 --process-timeout=300  --with-coverage --cover-package=alignak
  coverage combine
  cd .. && pep8 --max-line-length=100 --exclude='*.pyc' alignak/*
  pylint --rcfile=.pylintrc [...] -r no alignak/*
  pep257 --select=D300 alignak

Nosetest launches all test_*.py (unless they have a +x chmod), pep8 , pylint and pep257 checks python code.
Pylint is for now a very long line because we haven't done all rules yet. So, we only enable the rule we did


Commit :
~~~~~~~~
You should be ready to commit now, all new files and modified files are added in "stage"
If you look at the commit tree, you can notice more or less a pattern in commit message ::

  Enh|Fix|Add: <Generic word to describe> - <Specific word to descript>

Example::

  Enh: Tests - Clean unused imports

This is not a mandatory format to write commit. If you want to do it differently it's fine.
Always keep in mind that a commit message has to be clear enough.
Message like "fix", "try1", "update", "clean" are not really relevant to understand what's in the commit.

Create new tests :
~~~~~~~~~~~~~~~~~~
If you are fix a bug or adding a new feature you need to add test case.


Create pull request :
~~~~~~~~~~~~~~~~~~~~~
You feel like your fix / new feature is ready to be merge upstream? Time to create a pull request.
The pull request in the entry point for Alignak team' review process.
Keep in mind that we are humans and we usually are doing more that one thing at a time. So the clearer the pull request is the quicker it will be merged
Here are some hints to help reviewers ::

* Explain the issue you encountered, and how you fixed it (short description)
* Add test cases in a separate commit
* Link any Github issue it is related to (if you fix an issue for example)
* Mention any limitations of your imlpementation
* Mention any removal of supported feature

If you run the test previously you should see that Travis managed to build successfully. If not you will get an email.
Travis should passed in order to merge the pull request. Reviewers may not look at your pull request if build is broken.

.. tip:: You don't need such details for a typo / doc fix.

Release TODO list :
===================
Here are few thing to check when doing a release



.. _developer documentation: http://alignak.readthedocs.org/


