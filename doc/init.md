Sample init scripts and service configuration for kandled
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/kandled.service:    systemd service unit configuration
    contrib/init/kandled.openrc:     OpenRC compatible SysV style init script
    contrib/init/kandled.openrcconf: OpenRC conf.d file
    contrib/init/kandled.conf:       Upstart service configuration file
    contrib/init/kandled.init:       CentOS compatible SysV style init script

1. Service User
---------------------------------

All three startup configurations assume the existence of a "kandle" user
and group.  They must be created before attempting to use these scripts.

2. Configuration
---------------------------------

At a bare minimum, kandled requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, kandled will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that kandled and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If kandled is run with "-daemon" flag, and no rpcpassword is set, it will
print a randomly generated suitable password to stderr.  You can also
generate one from the shell yourself like this:

bash -c 'tr -dc a-zA-Z0-9 < /dev/urandom | head -c32 && echo'

Once you have a password in hand, set rpcpassword= in /etc/kandle/kandle.conf

For an example configuration file that describes the configuration settings,
see contrib/debian/examples/kandle.conf.

3. Paths
---------------------------------

All three configurations assume several paths that might need to be adjusted.

Binary:              /usr/bin/kandled
Configuration file:  /etc/kandle/kandle.conf
Data directory:      /var/lib/kandled
PID file:            /var/run/kandled/kandled.pid (OpenRC and Upstart)
                     /var/lib/kandled/kandled.pid (systemd)

The configuration file, PID directory (if applicable) and data directory
should all be owned by the kandle user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
kandle user and group.  Access to kandle-cli and other kandled rpc clients
can then be controlled by group membership.

4. Installing Service Configuration
-----------------------------------

4a) systemd

Installing this .service file consists on just copying it to
/usr/lib/systemd/system directory, followed by the command
"systemctl daemon-reload" in order to update running systemd configuration.

To test, run "systemctl start kandled" and to enable for system startup run
"systemctl enable kandled"

4b) OpenRC

Rename kandled.openrc to kandled and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
"/etc/init.d/kandled start" and configure it to run on startup with
"rc-update add kandled"

4c) Upstart (for Debian/Ubuntu based distributions)

Drop kandled.conf in /etc/init.  Test by running "service kandled start"
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon uitility.

4d) CentOS

Copy kandled.init to /etc/init.d/kandled. Test by running "service kandled start".

Using this script, you can adjust the path and flags to the kandled program by
setting the KANDLED and FLAGS environment variables in the file
/etc/sysconfig/kandled. You can also use the DAEMONOPTS environment variable here.

5. Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
