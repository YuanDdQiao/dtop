# dtop #

A web-based implementation of the excellent [htop project](http://htop.sourceforge.net).

## introduction ##

dtop is a tool that tries to deliver a large part of htop's realtime functionality to the webbrowser. It is currently very much a work in progress so please feel free to add features and improve!

## features ##

![Image](/doc/screenshot1.png?raw=true)
![Image](/doc/screenshot2.png?raw=true)

*   using Golang so no runtime requirements needed after compilation
*   only uses resources when the UI is accessed (and not a lot)
*   works on big and small, from Raspberry Pi to multi-processor systems
*	authentication support
*	cpu usage per core
*   basic info: hostname, distro, kernel
*	service status
*	memory- and swap-usage (overall and per process)
*	uptime
*	load avg
*	disk free/usage
*	users
*	basic process search functionality
*   mobile / tablet compatible ui

## installation ##

I'm currently working on hosting packages somewhere so precompiled binaries or packages are not yet available unfortunately. However, you can easily create these packages yourself by following the steps in development => distribution. If you only require the binary, you can use (Golang compiler required):

> export GOPATH=$(pwd)

> make run

or to just create the binary for an adhoc run:

> make build-linux-amd64

The binary should be in bin/linux-amd64

### run adhoc

You can start dtop as follows:

> ./bin/linux-amd64/dtop -c conf/default.json

Then point your webbrowser at http://localhost:12345 and you should see the dashboard.

The default configuration file (default.json) looks as follows:

	{
	    "Name": "my server",
	    "Description": "my description",
	    "StaticFolder": "/usr/local/share/dtop/static",
	    "Port": 12345,
	    "Services": [
	        { "Name": "ufw" },
	        { "Name": "ssh" },
	        { "Name": "cups" },
	        { "Name": "tomcat6" },
	        { "Name": "puppet" },
	        { "Name": "nfs-kernel-server" },
	        { "Name": "postgresql" }
	    ]
	 }

If you'd like to require authentication, you need to adapt the configuration file and add the users property. Here's an example:

	{
	    "Name": "my server",
	    "Description": "my description",
	    "StaticFolder": "/usr/local/share/dtop/static",
	    "Port": 12345,
	    "Users": [
	    	{ "Username": "hodor", "Password": "hodorhodor" }
	    ],
	    "Services": [
	        { "Name": "ufw" },
	        { "Name": "ssh" },
	        { "Name": "cups" },
	        { "Name": "tomcat6" },
	        { "Name": "puppet" },
	        { "Name": "nfs-kernel-server" },
	        { "Name": "postgresql" }
	    ]
	 }

When configured like this, a login dialog will appear before the dashboard is shown.

![Image](/doc/screenshot3.png?raw=true)

Please note that the password is not hashed yet...

### run as a daemon

Assuming you have the correct packages for your OS (see development => distribution). The deamon, dtopd, will be installed by default along with the package.

The service can then be controlled as usual:

> sudo service dtopd status|start|stop|restart

The configuration files are in /etc/dtop/

The static files are in /usr/local/share/dtop/static

The binary file (dtop) is in /usr/bin/dtop

## development ##

dtop is developed in [Go](http://golang.org) so you need the go compiler.

Clone the repo and cd into it.

> git clone https://github.com/ddierickx/dtop

> cd dtop

A Makefile is available with several helpful commands: test, format, build-all, dist-all, ... Note that they require that GOPATH and GOROOT are set.

### distribution ###

To ease the creation of packages for different OSes a Vagrantfile is available to create these packages. The VM can be found in the scripts/vm-distribution folder. If you have Vagrant and Virtualbox installed you can simply execute:

> vagrant up

This should create RPM and DEB packages for the i386 and amd64 in the dist folder.

## todo ##

*   provide pre-compiled binaries or packages (rpm/deb)
*   make (install) script based on install-dtopd.sh
*   re-add swap usage
*	user defined sorting iso. cpu percentage
*   a nice favicon/logo
*	hash passwords
*	processtree
*	basic (?) authentication
*	process kill feature
*	htop like keyboard shortcuts
*	implement Pri, Ni, Virt, Res, Shr, S, Time in process list.