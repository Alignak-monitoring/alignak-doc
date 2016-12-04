.. _Installation/rpm_packages:

=====================
Installation with RPM
=====================

You can find packages in a dedicated repository.

Create /etc/yum.repos.d/alignak.repo with the following content :

CentOS 7 ::

  [Alignak]
  name=Alignak CentOS-$releasever packages
  baseurl=http://alignak-monitoring.github.io/repos/centos/7/
  gpgcheck=1
  enabled=1
  gpgkey=https://alignak-monitoring.github.io/repos/alignak_pub.key
