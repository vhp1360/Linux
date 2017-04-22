<div dir=rtl>بنام خدا</div>

########top
- [Convert Commands](#convert-commands)
- [deal KVM with Command](#deal-kvm-with-command)


### Convert Commands
- from vdi to raw next to qcow2: 
```go
  VBoxManage clonehd disk.vdi disk.raw --format raw
  qemu-img convert -f raw disk.raw -O qcow2 disk.qcow2
```

[top](#top)

### deal KVM with Command
- Configuring Storage Pool
  1. Configure Pool or Create Defualt Pool:
  ```go
    virsh pool-define-as NPool dir - - - - '/Path/to/Pool/Place'
    virsh pool-list --all <- check all Storage in Pool
    virsh pool-build NPool && virsh pool-start NPool <- Make Pool Active and Start
    virsh pool-autostart NPool <- Make Pool as AutoStart
    virsh pool-info NPool
  ```
  2. Configure Storage:
  ```go
    qemu-img create -f raw /Path/to/Pool/Place/StorageName.img No.G
    qemu-img info /Path/to/Pool/Place/StorageName.img
  ```
  3.Configure Virtual Machine or Create Virtual Machine:
  ```go
    virt-install --name=OsName --disk path=/Path/To/Storage.img --graphics spice --vcpu=1 --ram=1024 --location=/Path/To/ISO.iso --network bridge=virbr0
    


