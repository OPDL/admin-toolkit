# ADMIN
This is a manual of the most important admin command line utilities and editors for Linux.

# SYSTEM
	lspci | grep -i nvidia
	uname -m
	cat /etc/*release
	**


Author: Georgy Georgiev 2015.
## Locale
https://www.thomas-krenn.com/en/wiki/Perl_warning_Setting_locale_failed_in_Debian

## PROCESSES
	 ps -p 27042 -o comm= # find name by id
	 ps aux | grep apache2 # find id by name
	 ps axjf # see a tree of ps
	 pgrep bash # find id by name
	 kill <PID> # stop pid
	 kill -KILL <pid> # force stop pid
	 kill -HUP <pid> # reload
	 killall firefox # kill all process with firefox
	 pkill -9 <name of process>

Utilities: top, htop, ps

## NETWORK
WiFi
	nmcli d wifi connect <WiFiSSID> password <WiFiPassword> iface wlan0

## PORTS
	netstat -ntlp | grep LISTEN
	netstat -plunt

## USERS
	adduser newuser
	visudo
	usermod -d /some/directory
	sudo -H -u otheruser bash -c 'echo "I am $USER, with uid $UID"
	su - <username> -c "<commands>"
  usermod -a -G groupName userName # add user to group
	cat /etc/group #list groups
	groups # see groups of current user

## STORAGE


	df -h # partitions size
	df -T # partitions formatting

	fdisk /dev/sda # partition
	mkfs.ext4 /dev/sda1 # format
	/etc/fstab

### Installing a new harddrive

[Link here](https://help.ubuntu.com/community/InstallingANewHardDrive)


### Mount
	  fdisk -l # list partions
	  mount -t auto -v /dev/sdb1 /mnt/mount_name
	  udisks --mount device_name
		umount -l /dev/sda1

- - -

## TASK SCHEDULING
### CRON

		$ crontab -l //list
		$ crontab -e //change/add
		$ crontab -r //remove
		Config file: /etc/crontab
		Default dirs:
			/etc/cron.hourly
			/etc/cron.daily
			...
		Example of a task:  */2 01-06 * 2 * rsync -avz <src> <dest>
		/**/
		Service: crond

+ MONITORING
	- MONIT
		Config file: $ /etc/monit.conf
		Log file: $ /var/log/monit
		Service: monit

## FIND
### GREP
	grep <search word> <where> <where>
	-r // recursively for directories
	--color // marks with color the search word
	-c // count
	-w // word

## EDIT
	## LESS
An editor which is a simpler and faster version of VI editor used just for reading files.

### VI
#### Forward Search
* / – search for a pattern which will take you to the next occurrence.
* n – for next match in forward
* N – for previous match in backward

### Backward Search
? – search for a pattern which will take you to the previous occurrence.
n – for next match in backward direction
N – for previous match in forward direction


	(escape)
	yy - cut line
	p - paste line
	:0 - begging of file
	:n - move to line n
	:$ - move to last line
	u - undo
	dd - delete line
	Ndd - delete N lines
	dNw - delete N words
	:.= - current line num
	:= - count of lines
	/ - search
	n - move search forward
	N - move search backward

	:r filename<Return>	 read file named filename and insert after current line
	:w <Return>	- write current contents to file named in original vi call
	:w newfile<Return>	- write current contents to a new file named newfile
	:12,35w smallfile<Return> -	write the contents of the lines numbered 12 through 35 to a new file named smallfile
	:w! prevfile<Return> -write current contents over a pre-existing file named prevfile

	insert
		ctrl f
		ctrl b
		ctrl d
		ctrl u

## CAT  
http://www.computerhope.com/unix/ucat.htm

	cat mytext.txt mytext2.txt > newfile.txt # concatenates 2 files in one
	cat myfile.txt # prints content of the file
	cat mytext.txt >> another-text-file.txt # append content of one file to another
	echo "My Shopping List" | cat - list.txt

## TAIL
http://www.computerhope.com/unix/utail.htm

 	tail -n 10  index.html #print last 10 lines
	tail -f access.log | grep 24.10.160.10 # monitor file

## HEAD
http://www.computerhope.com/unix/uhead.htm

	head -15 myfile.txt # first 15 lines
	head myfile.txt myfile2.txt # fisrt 10 lines of both
	head -n 4 *.txt # every file with extension .txt

## BACKING UP
### RSYNC
Remote
	  rsync -aAXv --exclude={"/dev/*","/proc/*","/sys/*","/tmp/*","/run/*","/mnt/*","/media/*","/lost+found"} -e 'ssh -i /path/to/private_key' root@SERVER_IP_ADDRESS:/* ~/backup/

Local
		/usr/bin/rsync -aAX --exclude={"/dev/*","/proc/*","/sys/*","/tmp/*","/run/*","/mnt/*","/media/*","/lost+found"} /*  /media/HDD/backups/backup_$(date +"%Y%m%d")

		Basic: rsync FLAGS/OPTIONS SRC DEST
		Example: $ rsync -avz /home/gooshan /backup
		Example: $ rsync -avz -e --rsh='ssh -p 1983' home/DIR username@server:/Folder
		Flags:
			--dry-run --incremental --log-file=FILE
			-avs - All objects, verbose output, do not allow remote shell to interpret characters;
			--delete will delete files at the target (destination), if they do not exist in the source.
			-h, which prints the rsync copy summary in a human-readable
			-l (lowercase L), when symlinks are encountered, recreate the symlink on the destination.
			--exclude=PATTERN exclude files matching PATTERN

## SECURITY
### CHECKSUM
		$ sha1sum <file>
		$ md5sum <file>
### SSH
#### Private key auth
HOST:
- Generate a private-public key pair on the host (ssh-keygen)
- In ~/.ssh/config on the host insert:
	Host <domain name or  IP>
		StrictHostKeyChecking no
		IdentityFile ~/.ssh/<private_key>
GUEST:
- Copy the public key in ~/.ssh/authorized_keys on the target. Make syre ~/.ssh is chmod 700 and the file itself is 600.
- In /etc/ssh/sshd_config on the target change optionally:
	PermitRootLogin no # no root access over ssh
	PasswordAuthentication no # no password auth over ssh
- Restart ssh
	sudo service ssh restart




+ BOOTING
	- SERVICE ON START
		$ chkconfig --levels 235 <service name> on
