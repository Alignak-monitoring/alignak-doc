.. _Installation/requirements:

===================
System requirements
===================

Some system requirements are needed to install Alignak:

   * python 2.7 or 3.5/3.6
   * python pip

The Python ``pip`` tool is necessary to install the Python dependencies needed by Alignak. Installing Python ``pip`` on different systems is :ref:`documented here <Installation/python_pip>`.

.. note:: for sure, the required Python packages should be made available in the Alignak repositories, but it is quite a lot of work and we do not have enough time for this... any help appreciated for this;)


User account
------------

Alignak is an application started from a privileged user account and it needs to use a low privileged user account. We recommend creating a user account identified as (in the default shipped configuration) *alignak* member of a group *alignak*.

 ::

      # Create alignak system user/group for Alignak application
      sudo addgroup --system alignak
      sudo adduser --system alignak --ingroup alignak


.. note:: the post installation Alignak script installed with distro packaging will create a default `alignak` user account if it does not yet exist on your system.

If you intend to use the Nagios checks plugins and if they are installed on your system, you should invite the user `alignak` into the `nagios` group. Most often, the installation of the Nagios checks plugins create a `nagios` user member of the `nagios` group...

 ::

      sudo usermod -a -G nagios alignak

Python version
--------------

If you intend to use Python 3, follow these recommendations.

Install the pip for Python 3::

    # For Debian-like
    $ sudo apt install python3-pip
    $ pip -V
    pip 8.1.1 from /usr/lib/python3/dist-packages (python 3.5)

    # For CentOS-like
    $ sudo yum install python34-pip

As defined in some PEP, the python command is mapped to Python 2. This may be a little tricky to use Python 3 per default. Thanks to `update-alternatives` it is easy to choose the default Python interpreter::

    # Nothing configured (it is often the case...)
    update-alternatives --list python
    update-alternatives: error: no alternatives for python

    $ sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1
    update-alternatives: using /usr/bin/python2.7 to provide /usr/bin/python (python) in auto mode

    $ sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.5 2
    update-alternatives: using /usr/bin/python3.5 to provide /usr/bin/python (python) in auto mode

    $ update-alternatives --list python
    /usr/bin/python2.7
    /usr/bin/python3.5

    # Python 3.5 is now the default one!
    $ python --version
    Python 3.5.2

    # Change this
    $ update-alternatives --config python
    There are 2 choices for the alternative python (providing /usr/bin/python).

      Selection    Path                Priority   Status
    ------------------------------------------------------------
    * 0            /usr/bin/python3.5   2         auto mode
      1            /usr/bin/python2.7   1         manual mode
      2            /usr/bin/python3.5   2         manual mode

    Press <enter> to keep the current choice[*], or type selection number:

    # Back to Python 2.7 if you choose 1
    $ python --version
    Python 2.7.12

.. note:: that you may need to choose Python 2 as the default interpreter for some operations on your system :/

.. warning:: `update-alternatives` is not easy to manage with CentOS7. You would rather choose a python version and get stucked with this version!


Python pip
----------

If your system does not yet include the Python ``pip`` tool, here is an installation reminder.


**For Debian-like Linux**
 ::

    # Python 2
    sudo apt install python-pip

    # Python 3
    sudo apt install python3-pip


**For RHEL-like Linux**

On a RHEL (CentOS, Oracle Linux,...), the Python pip installer tool is not included in the standard RHEL repositories but it is part of the EPEL repository. You must enable EPEL repository to install pip.

 ::

    sudo yum install epel-release
    sudo yum -y update

    # Python 2
    sudo yum install python-pip

    # Python 3
    sudo yum install python3-pip


    # On some older versions, it may be necessary to install extra compiler tools for the python *psutil* package::
    sudo yum install gcc python-devel


**For FreeBSD**::

    # Python 2.7
    sudo pkg install py27-pip

    # Python 3.6
    sudo pkg install py36-pip

