- ## Install ClamAV on Ubuntu #antivirus #linux #security
	- First we need to make sure the system is updated:
		- `# apt update`
	- Next install clamav together with the daemon:
		- `# apt install clamav clamav-daemon -y`
			- On Fedora it's `# dnf install clamav clamav-update`
	- Ensure it installed correctly:
		- `$ clamscan -V`
			- `ClamAV 0.103.2/26233/Thu Jul 15 07:31:54 2021`
	- By default, clamav-freshclam service will be enabled and running.
		- `$ sudo systemctl status clamav-freshclam`
			- Not needed on Fedora.
- ## Download and Update ClamAV Signature Database
	- Next step is updating the signature database:
		- To do this, we first need to stop the `clamav-freshclam` service, prior to updating the database:
			- `$ sudo systemctl stop clamav-freshclam`
		- Next, download and update the database manually using the following command:
			- `$ sudo freshclam`
				- **NOTE**: It is also possible to manually download the signature database from the ClamAV virus database mirror.
		- If you get the following output, then the database is updated:
			- ```
			  Tue Jul 13 04:15:19 2021 -> ClamAV update process started at Tue Jul 13 04:15:19 2021 
			  Tue Jul 13 04:15:19 2021 -> daily.cvd database is up to date (version: 25930, sigs: 4317819, f-level: 63, builder: raynman) 
			  Tue Jul 13 04:15:19 2021 -> main.cvd database is up to date (version: 59, sigs: 4564902, f-level: 60, builder: sigmgr) 
			  Tue Jul 13 04:15:19 2021 -> bytecode.cvd database is up to date (version: 331, sigs: 94, f-level: 63, builder: anvilleg)
			  ```
			- By default ClamAV signature database is updated automatically every hour, this behavior can be changed in freshclam configuration file /etc/clamav/freshclam.conf.
				- ClamAV use three virus definitions files such as main.cvd, daily.cvd, and bytecode.cvd these are kept in the directory /var/lib/clamav.
		- Next step is starting the freshclam daemon again:
			- `$ sudo systemctl start clamav-freshclam`
				- The output of the above command will indicate whether the virus signatures are up-to-date.
					- ```
					  clamav-freshclam.service - ClamAV virus database updater
					        Loaded: loaded (/lib/systemd/system/clamav-freshclam.service; enabled; vendor preset: enabled)
					        Active: active (running) since Fri 2021-07-16 01:41:20 UTC; 41s ago
					          Docs: man:freshclam(1)
					                man:freshclam.conf(5)
					                https://www.clamav.net/documents
					      Main PID: 65112 (freshclam)
					         Tasks: 1 (limit: 1073)
					        Memory: 2.0M
					        CGroup: /system.slice/clamav-freshclam.service
					                └─65112 /usr/bin/freshclam -d --foreground=true
					   Jul 16 01:41:20 li1129-224 systemd[1]: Started ClamAV virus database updater.
					   Jul 16 01:41:20 li1129-224 freshclam[65112]: WARNING: Ignoring deprecated option SafeBrowsing at /etc/clamav/freshclam.conf:22
					   Jul 16 01:41:20 li1129-224 freshclam[65112]: Fri Jul 16 01:41:20 2021 -> ClamAV update process started at Fri Jul 16 01:41:20 2021
					   Jul 16 01:41:20 li1129-224 freshclam[65112]: Fri Jul 16 01:41:20 2021 -> ^Your ClamAV installation is OUTDATED!
					   Jul 16 01:41:20 li1129-224 freshclam[65112]: Fri Jul 16 01:41:20 2021 -> ^Local version: 0.103.2 Recommended version: 0.103.3
					   Jul 16 01:41:20 li1129-224 freshclam[65112]: Fri Jul 16 01:41:20 2021 -> DON'T PANIC! Read https://www.clamav.net/documents/upgrading-clamav
					   Jul 16 01:41:20 li1129-224 freshclam[65112]: Fri Jul 16 01:41:20 2021 -> daily.cld database is up-to-date (version: 26233, sigs: 1961297, f-level: 90, builder: raynman)
					   Jul 16 01:41:20 li1129-224 freshclam[65112]: Fri Jul 16 01:41:20 2021 -> main.cld database is up-to-date (version: 61, sigs: 6607162, f-level: 90, builder: sigmgr)
					   Jul 16 01:41:20 li1129-224 freshclam[65112]: Fri Jul 16 01:41:20 2021 -> bytecode.cvd database is up-to-date (version: 333, sigs: 92, f-level: 63, builder: awillia2)
					  ```
		- For fresh installation, its recommended to start clamav daemon after ClamAV Virus Database (.cvd) file(s) installed. Now start clamav-daemon service to load database definitions to memory.
			- `$ sudo systemctl start clamav-daemon`
		- To verify clamd, check the ClamAV logs in /var/log/clamav/clamav.log
			- `tail /var/log/clamav/clamav.log`
- ## Testing ClamAV
	- For testing ClamAV, we can download a test virus to `/tmp` and scan using clamscan tool.
		- `$ cd /tmp`
		- `$ wget http://www.eicar.org/download/eicar.com`
		- `$ clamscan --infected --remove eicar.com`
			- ```
			  /tmp/eicar.com: Eicar-Test-Signature FOUND
			   /tmp/eicar.com: Removed.
			   ----------- SCAN SUMMARY -----------
			   Known viruses: 8553243
			   Engine version: 0.103.2
			   Scanned directories: 17
			   Scanned files: 1
			   Infected files: 1
			   Data scanned: 0.00 MB
			   Data read: 0.00 MB (ratio 0.00:1)
			   Time: 62.005 sec (1 m 2 s)
			   Start Date: 2021:07:16 02:08:29
			   End Date:   2021:07:16 02:09:31
			  ```
- ## Using Clamav
	- ClamAV configuration file is located at `/etc/clamav/clamd.conf`.
		- The configuration file allows to set scanning behavior, user name for clamd daemon (by default daemon is run by clamav), exclude directories from scanning, and much more.
	- ClamAV logs are stored in `/var/log/clamav/`, which contains information about each virus scan.
	- Scanning all files, from the current directory:
		- `$ clamscan -r /`
	- Scan files but only show infected files:
		- `$ clamscan -r -i /[path-to-folder]`
			- `--infected` - prints only infected files
			- `--remove` - removes infected files
			- `--recursive` - all directory and subdirectories in that path will be scanned
	- To scan infected files in a specific directory recursively and then remove them:
		- `$ clamscan --infected --remove --recursive /home/ubuntu/Desktop/`
	- To scan your web server and everything in the standard Apache document root, you scan any suspicious files and unwanted applications with the following command:
		- `$ sudo clamscan --infected --detect-pua=yes --recursive /var/www/html/`
			- _pua_ - Potential Unwanted Application
	- Scan files, but show only infected files without displaying OK files:
		- `$ clamscan -r -o /[path-to-folder]`
	- Scan files, but only send results of infected files to a new results file:
		- `$ clamscan -r /[path-to-folder] | grep FOUND >> /[path-folder]/[file].txt`
	- Scan and move infected files to a different directory path:
		- `$ clamscan -r --move=/[path-to-folder] /[path-to-quarantine-folder]`
- ## Setting up Automatic Scans with Email Notifications
	- Now let's add an entry to crontab:
		- `$ crontab -e`
	- I will add the following:
		- ```
		  MAILTO=example@mail.com
		  03 3 * * * /usr/bin/clamscan --recursive --infected --detect-pua=yes --no-summary /
		  ```
	- This will run a scan at 03:30 in the morning each day and only send an email notification if something infected is found.
- ## Installing ClamTK on Ubunti
	- **ClamTK** is a GUI for ClamAV.
		- **Installation:**
			- `$ sudo apt-get install clamtk`
