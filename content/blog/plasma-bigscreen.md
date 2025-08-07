+++
title = 'Plasma Bigscreen'
date = 2024-07-20T21:12:08+01:00
draft = false
[cascade]
	type = 'blog'
+++

> [!NOTE]
This guide was made when Bigscreen for Plasma 5 was the latest version.
It is currently being re-written to support Plasma 6 and
[unavalable to general users.](https://plasma-bigscreen.org/get/)


So you have a HTPC, and it needs an operating system, Windows 10 with the fullscreen
start menu works quite well as a launcher from couch distance to the screen
but a proper [10 foot interface](https://en.wikipedia.org/wiki/10-foot_user_interface)
would be better.
As a living room computer it also doesn't get turned on all too often so Windows updates are
most likely to interrupt film night too.

[Kodi](https://kodi.tv/) is the media player of choice for a HTPC as it integrates all
the things you want into a nice interface which can be navigated by keyboard and mouse
or a game controller (Xbox 360 in my case).

The following is built from [this](https://www.reddit.com/r/kde/comments/s6hlsk/plasma_bigscreen_running_on_kubuntu_2204_jamming/)
rough guide from Reddit.

[Plasma Bigscreen](https://plasma-bigscreen.org/) and kodi are both desktop environments for
Linux operating systems. However Kodi is not intended to be used as a launcher for other programs
and if logging into Kodi DE instead of launching it as a program it can be difficult to launch
other programs from within as it is not a proper window manager.

Starting with [Kubuntu](https://kubuntu.org/) and selecting normal installation rather than minimal
as some packages are left out which prevents Bigscreen from launching correctly. Install all the drivers
for the hardware it is installed on.

Now for a lot of dependencies:
```
sudo apt autopurge extra-cmake-modules
sudo apt install gcc make perl git build-essential cmake libkf5activities-dev libkf5activitiesstats-dev libkf5plasma-dev kirigami2-dev libkf5declarative-dev libkf5kcmutils-dev libkf5notifications-dev libkf5kio-dev libkf5wayland-dev plasma-workspace-dev qtmultimedia5-dev appstream qtbase5-dev qtchooser qt5-qmake qtbase5-dev-tools qttools5-dev libkf5networkmanagerqt-dev modemmanager-dev libkf5config-dev libkdecorations2-dev libqt5x11extras5-dev qtdeclarative5-dev libkf5guiaddons-dev libkf5configwidgets-dev libkf5windowsystem-dev libkf5coreaddons-dev gettext plasma-workspace-wayland kinit-dev qtquickcontrols2-5-dev 'python3-sphinxcontrib*' modemmanager-qt-dev
```
Note that extra cmake modules is being removed as the version pre-installed is too new.
Plasma Bigscreen is built on KDE Plasma 5, however Plasma 6 has been released and some packages have been updated
to support that unfortunately breaking compatibility. This is what makes it difficult to install onto Arch.

Install the correct version of extra cmake modules by manually cloning and installing this branch.
```
cd ~/Downloads
git clone --branch v5.116.0 https://invent.kde.org/frameworks/extra-cmake-modules
cd extra-cmake-modules
cmake .
make
sudo make install
```
Other dependencies that cannot be installed with apt or are already updated to Plasma 6:
```
cd ~/Downloads
git clone --branch kf5 https://github.com/KDE/kirigami-addons.git
cd kirigami-addons
cmake -B build -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Release
cmake --build build
sudo cmake --build build --target install

cd ~/Downloads
git clone --branch Plasma/5.27 https://invent.kde.org/plasma-mobile/plasma-settings.git
cd plasma-settings
cmake -B build -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Release
cmake --build build
sudo cmake --build build --target install
```

The rest can be installed using apt
```
sudo apt install plasma-nano plasma-remotecontrollers plasma-mobile
sudo apt install plasma-bigscreen
```
I have not had any luck using an Xbox controller to navigate through Bigscreen yet.
There is also a lot of cleaning up that needs to be done as the Kubuntu installer adds
a lot of programs that would ordinarily be useful for a desktop computer but just
clutter up the launcher here.

Additional tweaks include adding rules launch programs like RetroArch with the `--fullscreen` tag,
or Firefox always set to the maximum height and width of the display.

Other useful links
[Enable SSH on Debian](https://phoenixnap.com/kb/how-to-enable-ssh-on-debian)
[Add Firefox to apt](https://support.mozilla.org/en-US/kb/install-firefox-linux#w_install-from-your-distribution-package-manager-recommended)
