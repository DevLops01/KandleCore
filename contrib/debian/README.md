
Debian
====================
This directory contains files used to package kandled/kandle-qt
for Debian-based Linux systems. If you compile kandled/kandle-qt yourself, there are some useful files here.

## kandle: URI support ##


kandle-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install kandle-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your kandle-qt binary to `/usr/bin`
and the `../../share/pixmaps/kandle128.png` to `/usr/share/pixmaps`

kandle-qt.protocol (KDE)
