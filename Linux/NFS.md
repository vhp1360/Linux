<div dir="rtl">بنام خدا</div>
###### Top

- [GlusterFS](#glusterfs)
- [Samba](#samba)


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



[Top](#top)
###