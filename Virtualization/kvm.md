<div dir=rtl>بنام خدا</div>

########top
- [LibVirt Commands List](#libvirt-commands-list)
- [Convert Commands](#convert-commands)
- [deal KVM with Command](#deal-kvm-with-command)

[top](#top)

### [LibVirt Commands List](https://libvirt.org/apps.html#command)


### Convert Commands
- from vdi to raw next to qcow2: 
```go
  VBoxManage clonehd disk.vdi disk.raw --format raw
  qemu-img convert -f raw disk.raw -O qcow2 disk.qcow2
```

[top](#top)

### deal KVM with Command
- Configure Pool or Create Defualt Pool: _pool is kvm storage management_
```go
  virsh pool-define-as NPool dir - - - - '/Path/to/Pool/Place'
  virsh pool-list --all <- check all Storage in Pool
  virsh pool-build NPool && virsh pool-start NPool <- Make Pool Active and Start
  virsh pool-autostart NPool <- Make Pool as AutoStart
  virsh pool-info NPool
```
- Configure Storage:
```go
  qemu-img create -f raw /Path/to/Pool/Place/StorageName.img No.G
  qemu-img info /Path/to/Pool/Place/StorageName.img
```
- Configure Virtual Machine or Create Virtual Machine:
```go
  virt-install --name=GuestName --disk path=/Path/To/Storage.img --graphics spice --vcpu=1 --ram=1024 --location=/Path/To/ISO.iso \
    --network bridge=virbr0  <- Create New
  virt-install -n GuestName -r 11000 --os-type=... --os-variant=... --nographics --disk /Path/To/ImgFile,device=disk,bus=virtio \
    --vcpus=10 -w network=default,model=virtio --import <- Import Existing
```
- connect to Guest:
```go
  virsh --connect qemu:///system start UbuntuImg
```
- active Console Login
   1. first way: edit the /mnt/grub2/grub.cfg file and add ‘console=ttyS0‘ at the end of every line containing /vmlinuz \
      (the linux kernel).
      
   2. anotjer way:
      - first
        ```vim
          cp /etc/init/tty1.conf /etc/init/ttyS0.conf
        ```
      - then edit ttyS0.conf and change the line:
        ```vim
          exec /sbin/getty -8 115200 ttyS0 xterm
        ```
      - then edit /etc/default/grub:
        ```vim
          GRUB_CMDLINE_LINUX_DEFAULT=”console=ttyS0″
        ```
      - finally:
        ```vim
          update-grub2
        ```
   finaly:
  ```go
    virsh reboot GuestName
    virsh console GuestName
  ```
- Load Linux Guest as Part of Host Files: _sometimes it need console connection ability_
```go
  virsh destroy GuestName <- Destroy it
  virsh dumpxml | grep "source file=" <- find source Virtual Image Files
  kpartx -av /Path/to/Virtuals/ImgFile  <- it load(attach) Image file as directory in Host!!!
  mount /dev/mapper/loop... /mnt/
  umount /mnt
  kpartx -dv /Path/to/Virtuals/ImgFile

```
6. Find Guest IP Address:
```go
  virsh net-list
  virsh net-dhcp-leases NetWorkName
```
7. Add permanently guest to virsh:
```go
  virsh dumpxml GueatName <- to create related XML file that created in /etc/libvirt/qemu/
  virsh create /etc/libvirt/qemu/GuestName.xml
```
8. [deal Guest with command](#https://www.ibm.com/support/knowledgecenter/linuxonibm/liaat/liaatkvmvirsh.htm):
```go
  virsh list --all
  virsh start Guest{Name or ID}
  virsh reboot Guest{Name or ID}
  virsh shutdown Guest{Name or ID}
  virsh destroy Guest{Name or ID}
  virsh dominfo Guest{Name or ID}
  virsh save GuestID State-Name <- Create SnapShot
  virsh restore State-Name      <- Restore SnapShot
  virsh setmem GuestID Kilobytes
  virsh setmaxmem GuestID Kilobytes
  virsh setcpus GuestID No.
  virsh suspend GuestID
  virsh resume GuestID
```
9. [Attaching Storage](http://www.thegeekstuff.com/2015/02/add-memory-cpu-disk-to-kvm-vm):
```go
  qemu-img create -f raw MyNewDisck.img No.G
  virsh attach-disk GuestName --source /Path/to/MyNewDisck.img --target vdb --persistent
```

[top](#top)
