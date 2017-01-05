<div dir="rtl">بنام خدا</div>
###### Top

- [GlusterFS](#glusterfs)



#### Glusterfs
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
	2. be care on ssh Password Less config
	3. be care on IPTables
	4. be care on SeLinux
***
[Top](#top)
