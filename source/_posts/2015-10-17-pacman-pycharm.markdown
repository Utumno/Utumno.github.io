---
layout: post
title: "Pacman Pycharm"
date: 2015-10-17 00:36:13 +0300
comments: true
categories: 
---

Uninstall:

```
# find / -path /mnt/win -prune -o -iname "*Pycharm*"
/usr/bin/pycharm
/usr/share/applications/pycharm-community.desktop
/usr/share/pixmaps/pycharm.png
/opt/pycharm-community
/opt/pycharm-community/bin/pycharm.sh
/opt/pycharm-community/bin/pycharm.png
/opt/pycharm-community/bin/pycharm.vmoptions
/opt/pycharm-community/bin/pycharm64.vmoptions
/opt/pycharm-community/help/pycharmhelp.jar
/opt/pycharm-community/lib/pycharm.jar
/opt/pycharm-community/lib/src/pycharm-openapi-src.zip
/opt/pycharm-community/lib/src/pycharm-pydev-src.zip
/opt/pycharm-community/lib/pycharm-pydev.jar
/opt/pycharm-community/helpers/pycharm
/opt/pycharm-community/helpers/pycharm/pycharm_commands
/opt/pycharm-community/helpers/pycharm/pycharm_commands/pycharm_test.py
/opt/pycharm-community/helpers/pycharm/pycharm_run_utils.py
/opt/pycharm-community/helpers/pycharm/pycharm_setup_runner.py
/opt/pycharm-community/helpers/pycharm_generator_utils
/home/utumno/.PyCharm30
/home/utumno/.local/share/applications/jetbrains-pycharm.desktop
/home/utumno/.gnome/apps/jetbrains-pycharm.desktop
/var/lib/pacman/local/pycharm-community-3.1.1-1
/mnt/win

# pacman -R pycharm-community
checking dependencies...

Packages (1) pycharm-community-3.1.1-1

Total Removed Size:  192.85 MiB

:: Do you want to remove these packages? [Y/n] y
(1/1) removing pycharm-community

# find / -path /mnt/win -prune -o -iname "*Pycharm*"
/home/utumno/.PyCharm30
/home/utumno/.local/share/applications/jetbrains-pycharm.desktop
/home/utumno/.gnome/apps/jetbrains-pycharm.desktop
/mnt/win
```

Java:


```
# echo $JAVA_HOME
/opt/java
```

Using AUR:

<!-- more -->

```
$ git clone https://aur.archlinux.org/pycharm-professional.gitrofessional-4.5.4
Cloning into 'pycharm-professional'...
remote: Counting objects: 26, done.
remote: Compressing objects: 100% (17/17), done.
remote: Total 26 (delta 11), reused 24 (delta 9)
Unpacking objects: 100% (26/26), done.
Checking connectivity... done.


$ cd pycharm-professional


(master) $ makepkg
==> Making package: pycharm-professional 4.5.4-1 (Tue Sep 15 01:37:12 EEST 2015)
==> Checking runtime dependencies...
==> Missing dependencies:
  -> python-coverage
  -> python2-coverage
  -> ipython
  -> ipython2
==> Checking buildtime dependencies...
==> ERROR: Could not resolve all dependencies.
```

Ooops - needs python:

```
# pacman -S python-coverage
resolving dependencies...
looking for conflicting packages...

Packages (2) python-3.4.3-2  python-coverage-3.7.1-4

Optional dependencies for python
    python-setuptools
    python-pip
    sqlite [installed]
    mpdecimal: for decimal
    xz: for lzma [installed]
    tk: for tkinter [installed]
# pacman -S python2-coverage
resolving dependencies...
looking for conflicting packages...

Packages (1) python2-coverage-3.7.1-

# pacman -S ipython
resolving dependencies...
looking for conflicting packages...

Packages (5) python-decorator-4.0.2-1  python-packaging-15.3-1
             python-pexpect-3.3-1  python-setuptools-1:18.3.1-1
             ipython-4.0.0-2

Optional dependencies for ipython
    python-nose: for IPython's test suite

# pacman -S ipython2
resolving dependencies...
looking for conflicting packages...

Packages (3) python2-decorator-4.0.2-1  python2-pexpect-3.3-1  ipython2-4.0.0-2

Optional dependencies for ipython2
    wxpython: needed for ipython2 --gui=wx
    python2-nose: for IPython's test suite

# exit
```

Build:


```
utumno@mrsd-arch ~/_/pycharm-professional (master) $ makepkg
==> Making package: pycharm-professional 4.5.4-1 (Tue Sep 15 01:56:50 EEST 2015)
==> Checking runtime dependencies...
==> Checking buildtime dependencies...
==> Retrieving sources...
  -> Downloading pycharm-professional-4.5.4.tar.gz...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   154  100   154    0     0   1246      0 --:--:-- --:--:-- --:--:--  1262
100  153M  100  153M    0     0   852k      0  0:03:04  0:03:04 --:--:--  911k
  -> Found pycharm-professional.desktop
  -> Found pycharm-professional.install
  -> Found pycharm
==> Validating source files with sha256sums...
    pycharm-professional-4.5.4.tar.gz ... Passed
    pycharm-professional.desktop ... Passed
    pycharm-professional.install ... Passed
    pycharm ... Passed
==> Extracting sources...
  -> Extracting pycharm-professional-4.5.4.tar.gz with bsdtar
==> Entering fakeroot environment...
==> Starting package()...
==> Tidying install...
  -> Purging unwanted files...
  -> Removing libtool files...
  -> Removing static library files...
  -> Compressing man and info pages...
==> Creating package "pycharm-professional"...
  -> Generating .PKGINFO file...
  -> Adding install file...
  -> Generating .MTREE file...
  -> Compressing package...
==> Leaving fakeroot environment.
==> Finished making: pycharm-professional 4.5.4-1 (Tue Sep 15 02:02:11 EEST 2015)
```

Pacman install :


```
# cd /home/utumno/_/pycharm-professional
# ls
PKGBUILD				     pycharm-professional-4.5.4.tar.gz
pkg					     pycharm-professional.desktop
pycharm					     pycharm-professional.install
pycharm-professional-4.5.4-1-any.pkg.tar.xz  src
# pacman -U pycharm-professional-4.5.4-1-any.pkg.tar.xz
loading packages...
resolving dependencies...
looking for conflicting packages...

Packages (1) pycharm-professional-4.5.4-1

Total Installed Size:  318.10 MiB

:: Proceed with installation? [Y/n] y
(1/1) checking keys in keyring                     [######################] 100%
(1/1) checking package integrity                   [######################] 100%
(1/1) loading package files                        [######################] 100%
(1/1) checking for file conflicts                  [######################] 100%
(1/1) checking available disk space                [######################] 100%
(1/1) installing pycharm-professional              [######################] 100%

===>
===> Please set the Anti-aliasing font settings for Java app
===> adding the following line to the user's file ~/.bash_profile
===> (not in ~/.bashrc):
===>
===> export _JAVA_OPTIONS='-Dawt.useSystemAAFontSettings=setting'
===>
===> Replace 'setting' with on, lcd, gasp, etc. By default is
===> configured with lcd.
===>
===> Please read the following link for more options:
===> https://wiki.archlinux.org/index.php/Java_Runtime_Environment_Fonts
===>

```

Files:


```
# find / -type d \( -path /mnt/win -o -path /home/utumno/Downloads  \) -prune -o -iname "*Pycharm*"
/usr/bin/pycharm
/usr/share/applications/pycharm-professional.desktop
/usr/share/pixmaps/pycharm.png
/usr/share/licenses/pycharm-professional
/usr/share/licenses/pycharm-professional/PyCharm_license.txt
/opt/pycharm-professional
# /opt/pycharm-professional ...........
/home/utumno/.PyCharm30
/home/utumno/.local/share/applications/jetbrains-pycharm.desktop
/home/utumno/.PyCharm40
/home/utumno/.PyCharm40/config/eval/PyCharm3.evaluation.key
/home/utumno/.PyCharm40/config/pycharm30.key
/home/utumno/.gnome/apps/jetbrains-pycharm.desktop
/var/lib/pacman/local/pycharm-professional-4.5.4-1
/mnt/win
/root/pycharm-4.5.4
# rm /home/utumno/.gnome/apps/jetbrains-pycharm.desktop
# rm /home/utumno/.local/share/applications/jetbrains-pycharm.desktop
```

Note - wxpython:

```
# pacman -S wxpython2.8
resolving dependencies...
looking for conflicting packages...

Packages (3) wxgtk-3.0.2-4  wxpython-3.0.2.0-1  wxpython2.8-2.8.12.1-2

Total Download Size:    25.10 MiB
Total Installed Size:  135.53 MiB

:: Proceed with installation? [Y/n] y
:: Retrieving packages ...
 wxgtk-3.0.2-4-x86_64               6.6 MiB   807K/s 00:08 [###############################] 100%
 wxpython-3.0.2.0-1-x86_64         10.0 MiB   778K/s 00:13 [###############################] 100%
 wxpython2.8-2.8.12.1-2-x86_64      8.5 MiB   723K/s 00:12 [###############################] 100%
(3/3) checking keys in keyring                             [###############################] 100%
(3/3) checking package integrity                           [###############################] 100%
(3/3) loading package files                                [###############################] 100%
(3/3) checking for file conflicts                          [###############################] 100%
(3/3) checking available disk space                        [###############################] 100%
(1/3) installing wxgtk                                     [###############################] 100%
Optional dependencies for wxgtk
    webkitgtk2: for webview support [installed]
(2/3) installing wxpython                                  [###############################] 100%
(3/3) installing wxpython2.8                               [###############################] 100%
#

```
