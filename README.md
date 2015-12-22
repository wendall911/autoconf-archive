# Build current autoconf-archive for CentOS
This repository is derived from autoconf-archive-2012.09.08-1.el6.src.rpm

tmatsuu mysteriously removed this package, though it took ZERO effort to maintain in EPEL. Putting it here so folks can build for packages that require these macros.

## Building on Host OS
* If not already installed, install and configure system for building rpms. [Instructions](http://wiki.centos.org/HowTos/SetupRpmBuildEnvironment)
* Copy contents of this repo to ~/rpmbuild/SOURCES
* Fetch sources
  * spectool -g -R autoconf-archive.spec
* Build SRPM
  * rpmbuild -bs autoconf-archive.spec
* Install dependencies
  * sudo yum-builddep autoconf-archive-2012.09.08-1.el6.src.rpm
* Rebuild rpm
  * rpmbuild --rebuild autoconf-archive-2012.09.08-1.el6.src.rpm
* Install rpm
  * sudo rpm {-ihv | -U} ~/rpmbuild/RPMS/noarch/autoconf-archive-2012.09.08-1.el6.noarch.rpm

## Building Using Mock
* Install mock
  * [Notes on mock usage](https://fedoraproject.org/wiki/Using_Mock_to_test_package_builds)
* CentOS build
  * copy centos-6-i386.cfg and centos-6-x86_64.cfg to /etc/mock/
* Checkout autoconf-archive sources from Fedora Project
  * git clone https://github.com/wendall911/autoconf-archive.git
* Fetch sources
  * spectool -g -R -C ./ autoconf-archive.spec
* Build SRPM
  * sudo mock -r epel-6-x86_64 --buildsrpm --sources ./ --spec ./autoconf-archive.spec
* Copy generated SRPM to working directory
  * cp /var/lib/mock/epel-6-x86_64/result/autoconf-archive-2012.09.08-1.el6.src.rpm ./
* Install dependencies
  * sudo mock -r epel-6-x86_64 --installdeps autoconf-archive-2012.09.08-1.el6.src.rpm
* Build RPM
  * sudo mock -r epel-6-x86_64 rebuild autoconf-archive-2012.09.08-1.el6.src.rpm
* Copy built RPMS to working directory
  * cp /var/lib/mock/epel-6-x86_64/result/autoconf-archive-2012.09.08-1.el6.noarch.rpm ./
* Install autoconf-archive RPMS into chroot environment
  * sudo mock -r epel-6-x86_64 --install autoconf-archive-2012.09.08-1.el6.noarch.rpm

## Target Server Installation
* Install autoconf-archive RPM
  * rpm -ihv autoconf-archive-2012.09.08-1.el6.noarch.rpm
