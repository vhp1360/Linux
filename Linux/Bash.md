<div dir="rtl">بنام خدا</div>
##### top

- [Jobs](#jobs)
- [Users](#users)
- [IPV6](#ipv6)
- [VPN Routes](#vpn-routes)
- [Partitioning](#partitioning)
- [Files Operating](#files-operating)
- [NetWork](#network)
- [Scripting](#scripting)
- [BashProgramming](#bashprogramming)

##### Jobs
- \& :
- fg :
- bg :
- disown :
  `run command`,<kbd>Ctrl</kbd><kbd>Z</kbd>,`disown -h Job-ID`

[Top](#top)
##### Users
* Change username:
 - becare first find OldName user and group ID.
```go
  usermod -l NewName OldName
  groupmod -n NewName oldName
  usermod -d /home/NewName -m NewName
  find / -group OldGroupID -exec chgrp -h NewName {} \;
  find / -user OldUserID -exec chown -h Newname {} \;
```
* useradd
```go
  -e Account Expir Date
  -f Password Expire
  -M without Home Directory
  -d Specified Home Directory
  -N without Group
  -s specified shell
  -L without loggin permission
```

[Top](#top)
##### IPv6
* disabling IPv6
```go
  vim /etc/sysctl.conf
  net.ipv6.conf.all.disable_ipv6 = 1
  net.ipv6.conf.default.disable_ipv6 = 1
  sysctl -p
```


[Top](#top)
##### VPN Routes
* [coonect client to server which will connected to vpn:](http://unix.stackexchange.com/questions/237460/ssh-into-a-server-which-is-connected-to-a-vpn-service)
 + Public IP is 50.1.2.3
 +  Public IP Subnet is 50.1.2.0/24
 +  Default Gateway is x.x.x.1
 +  eth0 is device to gateway
```go
  ip rule add table 128 from 50.1.2.3
  ip route add table 128 to 50.1.2.0/24 dev eth0
  ip route add table 128 default via x.x.x.1
```
* Scans
```go
  IPTABLES -A INPUT -p tcp -i eth0 -m state --state NEW -m recent --set
  IPTABLES -A INPUT -p tcp -i eth0 -m state --state NEW -m recent --update --seconds 30 --hitcount 10 -j DROP
```

[Top](#top)
### Partitioning
- [Parted](#parted)


###### Parted
- add partition to existing free space(may raid) :
  1. `# parted` --> so you intered in (parted) mode.
  2. `select /dev/md100` --> inside of disk.
  3. `unit s print free` --> give fisrt and last sector number for free unpatitioned space on disk.
  4. `mkpart`  --> and go ahead and answer questions.
  5. `quit` --> goback to __bash__ mode.
  6. `mkfs. ... NewPartitionName`
  

[Top](#top)
#### Files Operating
- [rsync](#rsync)
- [find](#find)

########rsync:
```go
  rsync -a(~r(recusrsive)l(link)p(permission)o(owner)t(time modify)g(group)D(device,specials) -z(compress) \
  -h(human) -v(verbose) --include --exclude --remove-source-files --delete -u(updated) -d(sync directory) \
  --append(append to shorter files) -L(change symbol link to real) -W(copy file whole) --inplace(directly write) \
  --max-size --bwlimit
```
   
   1.Only Drive your Code:
   ```go
     rsync --drive-run ...
   ```
   2.Copy/Sync Files and Directory Locally:
   ```go
     rsync -zhv SourceFILE Destination/
     rsync -azhv SourceDIR Destination/
     rsync -azhv [ssh] SourceDir root@IP:/Path
     rsync -azhv [ssh] root@IP:/SourceDir Dest/
   ```
      1. -a: preserve permissions,attributes,symblinks, and recursion mode. ~ -rlptgoD \
-            this option need *Root* Permission.
      2. -z: compress files during transffering.
      3. [ssh]: by this you have secure copy.
      4. _r_ ~ --recursive
      5. _l_:copy symlinks as symlinks
      6. _p_:preserve permissions
      7. _t_:preserve modification times
      8. _g_ ~ --group
      9. _o_ ~ --owner
      10. _D_:same as --devices --specials
      11. _--devices_:preserve device files (super-user only)
      12. _--specials_:preserve special files
      
   3.Progrss bar:
   ```go
     rsync -azhv --progress SourceDir Destination/
   ```
   4.Include & Exclude:
   ```go
     rsync -azhv --include '*.R' --exclude '*e*.R' SourceDir/ Destination/
   ```
   5.delete source files or destination files
   ```go
     rsync -azhv --remove-source-files Source/ Destination/
     rsync -azhv --delete Source/ Destination/
   ```
      1. --delete: if folder exist in dest but not remain in source will be delete from Dest.
      2.you could use `--remove-source-files` instead of `mv` command.
  
   6.files size & Bandwidth limitation: will sync less or equeal than
   ```go
     rsync -azhv --max-size='20M' --bwlimit=100 ssh ...Source/ Destination/
   ```
   7.resumable:
   ```vim
     rsync --append ...
   ```
      1. depending your version may `--append-verify` command, that the same but with _checksum_ implementing.
      2. `--append-no-verify` is in contract above.
   8.commit partial copied:
   ```vim
     rsync --partial[-dir] ...
   ```
      1. `--partial-dir` used for directory.
      2. `-P` equeal to `--partial --progress`
   9.write directly: instead of default _rsync_ behavier, write updates directly to dest:
   ```vim
     rsync --inplace ...
   ```
   10.sync only Directories tree:
   ```vim
     rsync -d ...
   ```
   11.sync only existing files in destination:
   ```vim
     rsync --existing ...
   ```
   12.find different between source and destination:
   ```vim
     rsync -azvh -i ...
   ```
   13.Transfer the Whole File : it spped up transffering.
   ```vim
     rsync -azvh -W ...
   ```
########find
```go
  find multiple paths -name "Pattern" -type Type  -not -name "Pattern" --exec SomeCommands {} SomeCommands \;
```
 1.find in many Path
 2.find with specefic type: f-->file d-->directory
 3.find with extention: 
 ```vim
   find ... -type f \( -name "\*.cpp" -o -name "\*.csv" \)
 ```
 4.Sample of commands:
 ```go
   find . -iname "cpp" -type f --exec cp {} /home/root/Bkp/ \;
   find . -name "sample*.py" -type f --exec grep -B5 -A8 'function' {} \;
 ```
 5.find by midification time:
 ```vim
   find . -mtime 1  <--24 hours
   find . -mtime -5 <-- 5 days ago
 ```
 6.find and tar:
 ```go
   find . -type f \( -name "*.py" \) print0 | xargs -0 tar cvf myPyFile.tar  <--print0 handle space in file name.
 ```
 
 
[Top](#top)
#### NetWork
- ncat: to send data on machine port and listening.
  1. listening: `nc -l PortNo.`
  2. sending: `cat File | nc IP PortNo.`
- wget: to download
  1. simple code: `wget http:/.... or ftp://... `
  2. Username&Pass: 
  ```go
    wget http://userName:Password@Address
    wget --http-user=User --http-password=Password http://...
    wget --ftp-user=User --ftp-password=Password ftp://...
  ```
  3. Recursively of Web: `wget -r http://... or ftp://... `
  4. Resumable : `wget -c http://... or ftp://... `
  5. From Files: `wget -i /Path to Text File `

[Top](#top)
<div dir="rtl"></div>



### Scripting
- generate log of output:
```go
  exec 3>&1 4>&2
  trap 'exec 2>&4 1>&3' 0 1 2 3
  exec 1>log.out 2>&1
```
- define variable: `Var=Value` <-- be care _No Space_
- defualt value for variable: `[ "$Var" == "" ] && Var=DefaultValue`
- input Parameter : `Par1=$1` , ... <-- be care _No Space_
- input arguments:
 - `$@` : a list of all input arguments
 - `$#` : number of input arguments
- Array:
  - String to array: `IFS='sep' read -ra ListName <<< $1`
  - Length of Array: `${#ArrayName[@]}`
  
- Loop:
  - for:
  ```sh
    for 1 in {1..100};do ...;done
    for 1 in {1..100..10};do ...;done
    for ((i=0;i<No.;i++));do ...;done
  ```
- Variable dealing:
  - assign empty variable: simply use `VariableName=`
  - incrementally assign: `VariableName="$VariableName ..."` <-- be care about <kbd>Space</kbd>
- 

[Top](#top)
### BashProgramming