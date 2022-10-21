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

