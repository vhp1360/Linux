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



