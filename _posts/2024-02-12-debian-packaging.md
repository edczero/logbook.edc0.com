---
title: Debian Packaging Notes
author: edc0
categories: [Ubuntu, Debian-Packaging]
tags: [debian-packaging, git, deb]
render_with_liquid: false
---




## Building From Source:


### Gsettings Override:

dh_installgsettings is a debhelper program that is responsible for installing GSettings override files and generating appropriate dependencies on the GSettings backend.

example (gtk+ source): debian/libgtk-3-common.gsettings-override

Override example in debian rules:

```make
 #!/usr/bin/make -f

%:
    dh ${@}

override_dh_installgsettings:
    dh_installgsettings --priority=20   
```



Check dependency of the package: `dpkg-depcheck -d ./configure`



### Building two different configurations of the same package:

http://askubuntu.com/questions/317731/



Take a look at the qbittorrent package, it builds two versions of the package from the same source using the new dh syntax. Here's what it might look like in your case (untested):

```make
 #!/usr/bin/make -f
%:
    dh $@

override_dh_auto_configure:
    mkdir -p build-nox && cd build-default && ../configure --prefix=/usr
    mkdir -p build-gtk3 && cd build-gtk3 && ../configure --prefix=/usr --enable-gtk3-experimental

override_dh_auto_build:
    dh_auto_build --builddirectory build-default
    dh_auto_build --builddirectory build-gtk3

override_dh_auto_install:
    dh_auto_install --builddirectory build-default --destdir debian/inkscape-trunk
    dh_auto_install --builddirectory build-gtk3 --destdir debian/inkscape-trunk-gtk3

override_dh_auto_clean:
    dh_auto_clean --builddirectory build-default
    dh_auto_clean --builddirectory build-gtk3 
```







### Simple Meson build example:


```make
 #!/usr/bin/make -f

export DEB_BUILD_MAINT_OPTIONS = hardening=+all

%:
    dh $@ --with=scour

override_dh_auto_configure:
    meson build-deb --prefix=/usr

override_dh_auto_build:
    ninja -v -C build-deb
    cd po; intltool-update -p

override_dh_auto_install:
    DESTDIR=$(CURDIR)/debian/simple-scan ninja -v -C build-deb install

override_dh_installdocs:
    dh_installdocs --link-doc=simple-scan

override_dh_installchangelogs:
    dh_installchangelogs NEWS  
```

Sources: http://bazaar.launchpad.net/~ubuntu-desktop/simple-scan/ubuntu/view/head:/debian/rules
 https://anonscm.debian.org/git/pkg-gnome/graphene.git/tree/debian/rules



### Re-Packaging a DEB

Use folowing steps to repackage dep package:

```bash
#Extract deb package
pkg-deb -x <package.deb> <dir>

#Extract control-information from a package
dpkg-deb -e <package.deb> <dir/DEBIAN>

#Repack the deb
dpkg-deb -b <dir> <new-package.deb>
```


### Use checkinstall make install:
sudo (or su) make install is a bad idea. You might not be able to uninstall the software once installed and it might break when the qt libraries update. 

A better idea would be to install checkinstall and then install with it like this:

```
sudo checkinstall make install
```

checkinstall creates a deb package and then installs it using dpkg. This way you can uninstall the package using apt and track it with Synaptic (checkinstall'd packages have their own section).



usd-import and merge to debian example:
https://code.launchpad.net/~ahasenack/ubuntu/+source/bind9/+git/bind9/+merge/336719



Sources: [Autotool](http://abhinavsingh.com/getting-started-with-autotools-gnu-build-system-on-debian/)



### Git-Format patch:


```sh
#Commit your local changes using

git commit -a -m "specific message"
#Note : don't push this commit.

#Generate patch

git format-patch -s -n -1 HEAD
#it will generate 0001-.patch

#Revert back local commit
git reset --soft HEAD~1 + git reset HEAD *
#To delete commit but keep your work



git reset --hard HEAD~1
#to delete commit with your work
```



### Generate makefile.in from makfile.am:

```bash
autoreconf --install
```

https://stackoverflow.com/questions/2531827/what-are-makefile-am-and-makefile-in





