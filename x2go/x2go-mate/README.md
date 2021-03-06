# dockerfiles-fedora-x2go-mate

# To build:
Copy the sources to your docker host and build the container.
You must use an SELinux workaround for X2Go to be correctly installed within a container. See RHBZ #1222913 for more details:

	# setsebool virt_sandbox_use_all_caps 1
	# docker build --rm -t <username>/x2go-mate .
	# setsebool virt_sandbox_use_all_caps 0


The rest of these instructions assume you are OK with it using port 50000 on the host.

# To run with the default user account and its home dir:

	# docker run -d -p 50000:22 --privileged=true <username>/x2go-mate

# To run with the host's user accounts, sudo configuration, and home dirs:
	docker run -d -p 50000:22 --privileged=true --volume /etc/passwd:/etc/passwd --volume /etc/shadow:/etc/shadow --volume /etc/group:/etc/group --volume /etc/gshadow:/etc/gshadow --volume /etc/sudoers:/etc/sudoers --volume /home:/home <username>/x2go-mate

(This is limited to local user accounts & sudo configuration. If home dirs are under a different path than /home, mount that path instead (or in addition to) /home.)

# To test SSH connectivity (upon which X2Go is based):
	# ssh -p 50000 user@localhost
(Specify a system user account instead if running with system user accounts)

# To use:
Connect with X2Go Client, PyHoca-GUI, or PyHoca-CLI.
You can do a loopback connection (Host: localhost) or connect from another machine.

X2Go Client Settings:
* Host: hostname of the docker host
* Login: user
* SSH port: 50000
* Session type: MATE
(Specify a system user account instead if running with system user accounts)

PyHoca-CLI command:

	# pyhoca-cli --server <hostname-of-the-docker-host> -p 50000 -N -c MATE --user user --add-to-known-hosts
(Specify a system user account instead if running with system user accounts)
