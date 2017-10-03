.. _Installation/deb_packages:

=====================
Installation with DEB
=====================

Installing Alignak from ``deb`` packages is the recommended way. You can find packages in an Alignak dedicated repository. To proceed with installation, you must register the alignak repository and store its public key on your system.

This script is an example how-to adapt your system and install Alignak but it is to be adapted according to your OS:

::

    # create an apt source with the content according to your Linux distribution.
    echo "___Os___specific___source___" | sudo tee /etc/apt/sources.list.d/alignak.list
    sudo apt-get update
    # Get and store the Alignak repos public key
    wget -O- http://alignak-monitoring.github.io/repos/alignak_pub.key | sudo apt-key add -
    apt-key list
    sudo apt-get install alignak


According to your OS, use the following source in place of `___Os___specific___source___` in the former script example:

    - Debian 8 (jessie): ``deb [arch=amd64] http://alignak-monitoring.github.io/repos/deb jessie main``

    - Ubuntu 16.04 (xenial): ``deb [arch=amd64] http://alignak-monitoring.github.io/repos/deb xenial main``

    - Ubuntu 14.04 (trusty): ``deb [arch=amd64] http://alignak-monitoring.github.io/repos/deb trusty main``

    - Ubuntu 12.04 (precise) ``deb [arch=amd64] http://alignak-monitoring.github.io/repos/deb precise main``

