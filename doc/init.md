Sample init scripts and service configuration for g0coind
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/g0coind.service:    systemd service unit configuration
    contrib/init/g0coind.openrc:     OpenRC compatible SysV style init script
    contrib/init/g0coind.openrcconf: OpenRC conf.d file
    contrib/init/g0coind.conf:       Upstart service configuration file
    contrib/init/g0coind.init:       CentOS compatible SysV style init script

1. Service User
---------------------------------

All three Linux startup configurations assume the existence of a "g0coincore" user
and group.  They must be created before attempting to use these scripts.
The OS X configuration assumes g0coind will be set up for the current user.

2. Configuration
---------------------------------

At a bare minimum, g0coind requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, g0coind will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that g0coind and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If g0coind is run with the "-server" flag (set by default), and no rpcpassword is set,
it will use a special cookie file for authentication. The cookie is generated with random
content when the daemon starts, and deleted when it exits. Read access to this file
controls who can access it through RPC.

By default the cookie is stored in the data directory, but it's location can be overridden
with the option '-rpccookiefile'.

This allows for running g0coind without having to do any manual configuration.

`conf`, `pid`, and `wallet` accept relative paths which are interpreted as
relative to the data directory. `wallet` *only* supports relative paths.

For an example configuration file that describes the configuration settings,
see `contrib/debian/examples/g0coin.conf`.

3. Paths
---------------------------------

3a) Linux

All three configurations assume several paths that might need to be adjusted.

Binary:              `/usr/bin/g0coind`  
Configuration file:  `/etc/g0coincore/g0coin.conf`  
Data directory:      `/var/lib/g0coind`  
PID file:            `/var/run/g0coind/g0coind.pid` (OpenRC and Upstart) or `/var/lib/g0coind/g0coind.pid` (systemd)  
Lock file:           `/var/lock/subsys/g0coind` (CentOS)  

The configuration file, PID directory (if applicable) and data directory
should all be owned by the g0coincore user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
g0coincore user and group.  Access to g0coin-cli and other g0coind rpc clients
can then be controlled by group membership.

3b) Mac OS X

Binary:              `/usr/local/bin/g0coind`  
Configuration file:  `~/Library/Application Support/G0CoinCore/g0coin.conf`  
Data directory:      `~/Library/Application Support/G0CoinCore`
Lock file:           `~/Library/Application Support/G0CoinCore/.lock`

4. Installing Service Configuration
-----------------------------------

4a) systemd

Installing this .service file consists of just copying it to
/usr/lib/systemd/system directory, followed by the command
`systemctl daemon-reload` in order to update running systemd configuration.

To test, run `systemctl start g0coind` and to enable for system startup run
`systemctl enable g0coind`

4b) OpenRC

Rename g0coind.openrc to g0coind and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
`/etc/init.d/g0coind start` and configure it to run on startup with
`rc-update add g0coind`

4c) Upstart (for Debian/Ubuntu based distributions)

Drop g0coind.conf in /etc/init.  Test by running `service g0coind start`
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon utility.

4d) CentOS

Copy g0coind.init to /etc/init.d/g0coind. Test by running `service g0coind start`.

Using this script, you can adjust the path and flags to the g0coind program by
setting the G0COIND and FLAGS environment variables in the file
/etc/sysconfig/g0coind. You can also use the DAEMONOPTS environment variable here.

4e) Mac OS X

Copy com.g0coin.g0coind.plist into ~/Library/LaunchAgents. Load the launch agent by
running `launchctl load ~/Library/LaunchAgents/com.g0coin.g0coind.plist`.

This Launch Agent will cause g0coind to start whenever the user logs in.

NOTE: This approach is intended for those wanting to run g0coind as the current user.
You will need to modify com.g0coin.g0coind.plist if you intend to use it as a
Launch Daemon with a dedicated g0coincore user.

5. Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
