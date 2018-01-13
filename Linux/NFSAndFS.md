<div dir="rtl">بنام خدا</div>

###### Top
- [Network File Sharing](#network-file-sharing)
	- [GlusterFS](#glusterfs)
	- [Samba](#samba)
	- [VSFTP](#vsftp)
- [Files System](#files-system)
	- [ISO](#iso)


# Network File Sharing
### Glusterfs
1. Installation : glusterfs-server gluster-client
2. Make a Partition:
	1. if you have new partition, nothing to do.
	2. if you haven't free partition: pass below steps to create a new partition of existing space!
		1. `touch GlusterFSfile`
		2. `dd if=/dev/zero of=GlusterFSfile bs=8192 count=10000000 --> to create 81G partition`
		3. `mkfs.xfs GlusterFSfile`
		4. `mount -t xfs GlusterFSfile /wherever`
	3. service glusterd start on all machine
	4. `gluster peer probe Other Machines` --> run all machines.
	5. `gluster peer status or list` --> to check .
	6. `gluster pool list` --> also for another layer check.
	7. `gluster volume create GlusterFSDriveName replica No. transport tcp,rdma ThisMachine:/path to GlusterFsfileMountPoint NextMachine:\
	/path to GlusterFsfileMountPoint ...` --> only on one machine.
	8. `gluster volume start GlusterFSDriveName`
	9. finally you need mount new GlusterFSPartition on each machine or client and check permissions.\
	`mount -t glusterfs MachineName:/GlusterFSDriveName /Path to Mount Point`
	10. if you need remove GlusterFSDriveName --> `gluster volume stop GlusterFSDriveName && gluster volume delete GlusterFSDriveName`
	11. if you need remove machine --> `gluster peer detach MachineName`

***
>	1. be care on hosts file: you need complete it and fill loopback IP line with itself machine name too.
>	2. be care on ssh Password Less config
>	3. be care on IPTables
>	4. be care on SeLinux
	
***
[Top](#top)
### Samba
- Securing from sambaCry attack.
   - Which IP:
   ```vim
     hosts deny = ALL
     hosts allow = 192.168.1.
   ```
   - Which version
   ```vim
     server min protocol = SMB2_10
     client max protocol = SMB3
     client min protocol = SMB2_10
   ```
- [Samba And SeLinux](http://danwalsh.livejournal.com/14195.html):
```vim
  setsebool -P samba_domain_controller on
  semanage fcontext -a -t samba_share_t '/<shared path>(/.*)?'
  restorecon -R /<shared path>
```
- [Samba And Iptables](https://wiki.centos.org/HowTos/SetUpSamba#head-26340a1a2c9cb4d46d51b9429fd030239b57feb4):
```vim
-A INPUT -s 192.110.133.0/24 -p tcp -m state --state NEW -m tcp -m multiport --dports 139,445 -j ACCEPT
-A INPUT -s 192.110.133.0/24 -p udp -m state --state NEW -m udp -m multiport --dports 137:138,445 -j ACCEPT
```
- Sharing Folder: to keep more security, we should set chmod ---- and also setfacl, the Kvm user is `qemu`.\
	if we revoke other user acess, we should have same username userid gourpname and groupid.

- mounting:
   - by command:
   ```vim
     mount -t cifs //servername/sharename  /media/windowsshare  username=UserName
   ```
   - put it in fstab:
   ```vim
     //servername/sharename  /media/windowsshare  cifs  username=UserName,password=Password,iocharset=utf8,sec=ntlm  0  0
   ```
   
### VSFTP
1-
```vim
	yum install vsftpd
	cp /etc/vsftpd/vsftpd.conf /etc/vsftpd/vsftpd.conf.default
```
2- two type of installation:

	1- Without Anonymouse User:
	
	```vim
		anonymous_enable=NO       	# disable  anonymous login
		local_enable=YES						# permit local logins
		write_enable=YES						# enable FTP commands which change the filesystem
		local_umask=022		     			# value of umask for file creation for local users
		dirmessage_enable=YES	  		# enable showing of messages when users first enter a new directory
		xferlog_enable=YES					# a log file will be maintained detailing uploads and downloads
		connect_from_port_20=YES  	# use port 20 (ftp-data) on the server machine for PORT style connections
		xferlog_std_format=YES    	# keep standard log file format
		listen=YES   								# only one of listen or listen_ipv6 sould uncommented ,this for running in standalone mode
		listen_ipv6=YES		        	# vsftpd will listen on an IPv6 socket instead of an IPv4 one
		pam_service_name=vsftpd   	# name of the PAM service vsftpd will use
		userlist_enable=YES  	    	# enable vsftpd to load a list of usernames
		tcp_wrappers=YES  					# turn on tcp wrappers
		chroot_local_user=YES				# for OS user this only loging users to their home and they have regulare access like SSH
		allow_writeable_chroot=YES	#		
		ssl_enable=YES							# more secure and we use this, when try connect from terminal we should use 'sftp' command
		ssl_tlsv1_2=YES
		ssl_sslv2=NO
		ssl_sslv3=NO
		allow_anon_ssl=NO
		force_local_data_ssl=YES
		force_local_logins_ssl=YES
		require_ssl_reuse=NO
		ssl_ciphers=HIGH
		debug_ssl=YES
		rsa_cert_file=/Path/vsftpd.pem
		rsa_private_key_file=/Path/vsftpd.pem
		pasv_min_port=40000					# to active passive mode
		pasv_max_port=50000
	```
	- if you'd like restrict all user from _ftp_ you:
	```vim
		userlist_enable=YES                   # vsftpd will load a list of usernames, from the filename given by userlist_file
		userlist_file=/etc/vsftpd.userlist    # stores usernames.
		userlist_deny=NO	
	```
	- SELinux:
	```vim
		setsebool -P ftp_home_dir on								# in redhat before 7
		setsebool -P tftp_home_dir on								# in redhat 7
		semanage boolean -m ftpd_full_access --on
	```
	- IPTables:
	```vim
		-A INPUT -p tcp -m tcp -m state --state NEW -m nultiport --dports 20,21 -j ACCEPT
	```
	
	2/2-
	
[Top](#top)
# Files System
### ISO
- How to create usb bootable
```vim
  fdisk -l <-- to find device assume sdc
  umount /dev/sdc
  mkdosfs -I /dev/sdc -F 32
  isohybrid /path/to/ISO.iso
  dd if=/path/to/ISO.iso of=/dev/sdc bs=4; sync; eject /dev/sdc
```

[Top](#top)
###
