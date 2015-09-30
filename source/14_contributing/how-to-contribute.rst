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

Step by step contribution example :
===================================



Ways to contribute :
=====================

    * help on the documentation `Alignak documentation`_
    * help on updating this web site
    * help on tracking and fixing bugs, `Alignak is on github`_ to make it easy!
    * coding new features, modules and test cases
    * performance profiling of the various daemons, interfaces and modules
    * providing references for large installations
    * submitting ideas to Alignak Ideas
    * responding to questions on the forums

.. tip::  Guidelines and resources are described for users in the first section and power users and developers in the second section.


Alignak Guidelines for developers and power users :
====================================================

Guidelines that should be followed when contributing to the code

    * Guidelines - :ref:`Hacking the code <development/hackingcode>` [Examples of Alignak programming]
    * Guidelines - :ref:`How to add a new WebUI page <packages/webui/webui-devel>`
    * Guidelines - :ref:`Test driven development <development/test-driven-development>` [How to create and run tests]
    * Guidelines - :ref:`Programming rules <development/programming-rules>` [Style, technical debt, logging]
    * Informational - :ref:`Feature planning process and release cycle <about/features-and-release-cycle>`

Resources for developers and power users

    * Development - Collaborative code repository on `Alignak github`_
    * Development - Bug tracking on `Alignak github`_
    * Development - Automated test and integration on `Alignak Jenkins server`_
    * Development - The forums are also a good medium to discuss issues `Support Forums`_
    * Development - Developer Mailing list - `Register or search the alignak-devel Mailing list`_

For bug hunting and programming, you will need to look at the “How to hacking" tutorial page.

GitHub offers great facilities to fork, test, commit, review and comment anything related to Alignak. You can also follow the projects progress in real time.

There is a development mailing list where you can join us. Come and let us know what you think about Alignak, propose your help or ask for it. :)

Thank you for your help in making this software an open source success.


.. _developer documentation: http://alignak.readthedocs.org/

.. _Shinken issues and bug reports: https://github.com/Alignak-monitoring/alignak/issues?sort=created&direction=desc&state=open
.. _Register or search the shinken-devel Mailing list: https://lists.sourceforge.net/lists/listinfo/alignak-devel
.. _Alignak github: https://github.com/Alignak-monitoring/alignak/issues?sort=created&direction=desc&state=open
.. _Alignak documentation: http://alignak.readthedocs.org/
.. _Alignak Jenkins server: https://test.savoirfairelinux.com/view/Alignak/
.. _Alignak is on github: https://github.com/Alignak-monitoring/alignak/
.. _Support Forums: http://...

