# utils
Disorganised utilities written for varying purposes.

## dexec (Docker-Exec)

Utility to quickly run commands on a docker container.

### Arguments and defaults

* Show usage and exit: -h | --help
* Invoke the command as a specific user ID or name: -u 0 | --uid 0 | --user 0 | --uid=0 | --user=0
* Invoke the command (if any) using a specific shell: -s /bin/sh | --shell /bin/sh | --shell=/bin/sh
* Look in a specific Docker stack for the given container: -S [stack name] | --stack [stack name] | --stack=[stack-name]
* Invoke the command on a docker container matching the given name fragment: -n [fragment] | --name [fragment] | --name=[fragment]
* Stop parsing any arguments: --

After any and all arguments, specify a name fragment if one wasn't specified already.

### Usage examples

On a host with a "nextcloud" docker stack deployed, which contains a service named "server":

	% dexec -s /bin/bash -S nextcloud --name=server --user www-data
	(bash prompt for the www-data user in whichever container is named 'server' within the 'nextcloud' stack)

On a host with a container named "wordpress":

	% dexec wordpress
	(POSIX sh prompt for the root user in whichever container has 'wordpress' in its name)

On a host with multiple containers running with "nextcl" in the name:

	% dexec nextcl
	(a listing of all containers matching fragment 'nextcl', asking for a more specific name)

## pushbeat

Utility intended to be run via crontasks that sends a heartbeat back to a given Uptime-Kuma install

### Installation

* Copy `pushbeat` to `/usr/local/bin/`
* Copy `pushbeat.conf` to `/etc/` or `/usr/local/etc/`
* `touch /var/log/pushbeat /var/log/pushbeat.err`
* Ensure the user you intend to run pushbeat as, can write to the above logfiles.

### Configuration

Edit `pushbeat.conf` with your FQDN (address where uptime-kuma is hosted, WITHOUT protocol) and KEY (a unique token given to you by Uptime-Kuma when you create an incoming HTTP monitor)

### Usage

Edit your crontab to include the following:

	* * * * * /usr/local/bin/pushbeat

On other operating systems, such as macOS, you may need to configure cron a different way.

## install-alpine

Alpine Linux install script specific to Hetzner dedicated servers. It will WIPE your server's drives before doing so. Note that the script does not configure any form of RAID; it assumes your server is equipped and configured with hardware RAID already.

### Usage

Edit the script to update several variables:

* Set IPSUB to the IPv4 subnet size, if you know that it is not /27 (it usually is)
* Set DIST_VER to the version of Alpine Linux you'd like to install (not including point-releases, ie 3.16.1 would simply be 3.16)
* Set DIST_APK_VER to the current version of the apk-tools-static package for your chosen release
* Set DIST_MIRROR if you have a preferred mirror
* Set INST_FEAT to include the list of features to be included by mkinitfs
* Set INST_MODS to include the list of kernel modules required for the system to boot

Make sure you have a username and password for the Hetzner Robot API, and export them to your shell environment as follows:

	% export ROBOT_USERNAME=my_username
	% export ROBOT_PASSWORD=my_password

Then execute the script:

	% ./install-alpine

## minimotd

Alpine Linux based MOTD script, to be run at startup by the `local` service.

### Installation

Ensure that the `local` service is set to run with the command `rc-update add local default`, then copy `minimotd` to `/etc/local.d/01_update_motd.start`

Then create and edit `/etc/motd.servdesc` with information about the machine in question, for example warnings about the software running on it, or any maintenance instructions that must be followed.

## node-down & node-up

Simple scripts to bring down or bring up the local docker node, useful for maintenance windows.

### Usage

To bring down a node, simply run `node-down`. A `watch` session will begin, showing all docker services and their current state. Press ctrl-c to exit, and a second `watch` session will begin, showing all running docker containers, as sometimes a service will be at 0 replicas, but still shutting down. Press ctrl-c to exit.

To bring up a node, simply run `node-up`. A `watch` session will begin, showing all docker services. Press ctrl-c if you don't want to supervise service startup, or once all services have settled.

