.. _contributing/how-to-contribute:

===================================
A step by step contribution example
===================================

The process to contribute to Alignak is quite simple. However, depending on what you are planning to do, the most efficient way to do it may vary.

Very simple fix with Github
---------------------------
If you have a very small to do (typo, doc, one file commit), you'd better use GitHub. You can click edit in the Alignak repository.
It will fork the repository for you and let you edit the file through the Web interface.
Once you picked a good commit message (see below for commit message habits) you can push it in a new branch (see below for branch name habits).
Finally, you can create a new pull request to the Alignak repository (still with GitHub UI)


Simple fix
----------

Checkout new branch
~~~~~~~~~~~~~~~~~~~

Once you have forked the repository and added remote (see above) you can start a new branch ::

   git checkout -b Add_ponies_and_rainbows

Here your new branch is ``Add_ponies_and_rainbows``.

Good habits is to name the branch as:

   * ``fix-#123`` for a branch fixing the issue *#123*.
   * ``feature`` for a branch implementing a new *feature*

You can now start editing the files...

Coding standards
~~~~~~~~~~~~~~~~

Alignak application source code is complying to the coding best practices edicted in `PEP 8`_ and `PEP 257`_. All the source code must also comply to some lint rules that are statically analysed.

.. _PEP 8: http://www.python.org/dev/peps/pep-0008/
.. _PEP 257: http://www.python.org/dev/peps/pep-0257/

Running the same analysis as the one that are executed during a Travis build is an easy stuff. Run the following script in the repository main directory::

      # Static code analysis
      # -- pycodestyle (former pep8)
      pycodestyle --max-line-length=100 --exclude='*.pyc,carboniface.py' alignak/*
      # -- pylint
      pylint --rcfile=.pylintrc -r no alignak
      # -- pep257
      pep257 --select=D300 alignak

Some good links about Python code style:

- `Guide to Python <http://docs.python-guide.org/en/latest/writing/style/>`_ from Hitchhiker's
- `Google Python Style Guide <https://google.github.io/styleguide/pyguide.html>`_


Run Alignak
~~~~~~~~~~~
The *dev* directory of the repository includes several useful scripts to run Alignak daemons on your development platform. The scripts names are self explanatory and the scripts are commented.

Installing Alignak and its default configuration will create an environment almost identical to the one you will find on your production server.
See the `how it works <howitworks/run_alignak>`_ chapter for more information.


Tests
~~~~~

Before committing (unless you know that you are pushing unfinished stuff) you should create or update some tests. Whether you fix a bug or add a new feature you need to add some test cases.

The Alignak test environment is :ref:`described in details here <contributing/testing>`_.

An example to run the whole test suite::

      cd tests
      pytest --verbose --durations=10 --no-print-logs --cov=alignak --cov-config .coveragerc test_*.py

      # A more simple test run (without verbose and code coverage)
      pytest test_*.py

      # A single test
      pytest test_my_tests.py

If you have enabled Travis on your fork (recommended) you will receive Travis notifications about the tests results once you pushed some commits to your repository. Else you should browse the Travis builds on the Travis CI.


Commit
~~~~~~

You should be ready to commit now, all new files and modified files are added in "stage". If you look at the commit tree, you can notice more or less a pattern in commit message ::

      Enh|Fix|Add: <Generic word to describe> - <Specific word to describe>

Example::

      Enh: Tests - Clean unused imports

This is not a mandatory format to write commit. If you want to do it differently it's fine.
Always keep in mind that a commit message has to be clear enough.
Message like "fix", "try1", "update", "clean" are not really relevant to understand what's in the commit.


Create pull request
~~~~~~~~~~~~~~~~~~~
You feel like your fix / new feature is ready to be merged upstream? It is time to create a pull request. The pull request is the entry point for Alignak team's review process.

Keep in mind that we are humans and we usually are doing more that one thing at a time. So the clearer the pull request is the quicker it will be merged
Here are some hints to help reviewers ::

    * Explain the issue you encountered, and how you fixed it (short description)
    * Add test cases in the main commit (better) or in a separate commit
    * Link any GitHub issue it is related to (if you fix an issue for example)
    * Mention any limitations of your implementation
    * Mention any removal of supported feature

If you run the tests previously you should see that Travis managed to build successfully. If not you will get an email from the Travis engine to informa about the tests results.

No pull request will be merged upstream until all Travis tests pass and until one of the reviewer will accept the Pull Request. Reviewers may not look at your pull request if the build is broken.

.. tip:: You don't need such details for a typo / doc fix.
