
Debian
====================
This directory contains files used to package g0coind/g0coin-qt
for Debian-based Linux systems. If you compile g0coind/g0coin-qt yourself, there are some useful files here.

## g0coin: URI support ##


g0coin-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install g0coin-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your g0coin-qt binary to `/usr/bin`
and the `../../share/pixmaps/g0coin128.png` to `/usr/share/pixmaps`

g0coin-qt.protocol (KDE)

