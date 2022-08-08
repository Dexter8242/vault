title:: Guides/IT/Using RPM

- # RPM Package Manager
	- Querying a package:
	  collapsed:: true
		- `rpm -q BitTorrent-5.2.2-1-Python2.4.noarch.rpm`
			- Additional information about the program as well:
				- `rpm -qi BitTorrent-5.2.2-1-Python2.4.noarch.rpm`
		- Check a packages signature:
		  id:: 62f0e26e-6559-47c4-8940-0be61489cf89
			- `rpm --checksig package.rpm`
	- Installing a package:
	  id:: 62f0dfb2-be2a-4827-b066-4c9f5e72c45e
	  collapsed:: true
		- `# rpm -ivh BitTorrent-5.2.2-1-Python2.4.noarch.rpm`
			- `-i` - Install a package
			- `-v` - Verbose output
			- `-h` - Print hashmarks as the package is unpacked
	- Installing a package without dependencies:
	  collapsed:: true
		- `# rpm -ivh --nodeps package.rpm`
			- Skips dependencies check, useful if you know all the dependencies are already installed.
	- List recently installed packages:
	  collapsed:: true
		- `# rpm -qa --last`
	- List all installed packages:
	  collapsed:: true
		- `rpm -qa`
	- Upgrade a package:
	  collapsed:: true
		- `# rpm -Uvh package.rpm`
	- List what package a file belongs to:
	  collapsed:: true
		- `rpm -qf /usr/bin/htpasswd`
	- Get information about a package prior to installing:
	  collapsed:: true
		- Use `rpm -qip` (**query info package**).
	- Query documentation for a package:
	  collapsed:: true
		- `rpm -qdf /usr/bin/netstat` (**query document file**)
	- Verify the integrity of a package:
	  collapsed:: true
		- `rpm -Vp package.rpm`
	- To see what files ship with a certain package:
	  collapsed:: true
		- `rpm -ql BitTorrent-5.2.2-1-Python2.4.noarch.rpm`
	- To see what configuration files (if any) ship with a certain package:
	  collapsed:: true
		- `rpm -qc BitTorrent-5.2.2-1-Python2.4.noarch.rpm`
	- To show what packages require a certain package as it's dependency:
	  collapsed:: true
		- `rpm -q --whatrequires BitTorrent-5.2.2-1-Python2.4.noarch.rpm`
	- List what dependencies a package requires:
	  collapsed:: true
		- `rpm -qpR BitTorrent-5.2.2-1-Python2.4.noarch.rpm`
	- Upgrading a package:
	  collapsed:: true
		- `# rpm -Uvh BitTorrent-5.2.2-1-Python2.4.noarch.rpm`
	- Remove a package:
	  collapsed:: true
		- `# rpm -e nx`
	- Verifying all packages integrity:
	  collapsed:: true
		- `# rpm -Va`