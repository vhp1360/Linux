<div dir="rtl">بنام خدا</div>

- [Enviroments](#enviroments)
- [Jobs](#jobs)
- [Users](#users)
  - [Change username](#changeusername)
  - [useradd](#useradd)
  - [usermod](#usermod)
- [IPV6](#ipv6)
  - [disabling IPv6](#disablingipv6)
- [VPN Routes](#vpn-routes)
- [Partitioning](#partitioning)
  - [Parted](#parted)
  - [dd](#dd)
- [Files Operating](#files-operating)
  - [rsync](#rsync)
  - [find](#find)
- [Scripting](#scripting)
  - [Logging](#logging)
  - [Variable](#variable)
  - [Input Prameters](#input-parameters)
  - [Loop](#loop)
  - [Conditional](#conditional)
- [Text Processing](#text-processing)
  - [REGEX](#regex)
    - [Regex Table](#regex-table)
  - [AWK](#awk)
  - [CUT](#cut)
  - [SED](#sed)
  - [Sort](#sort)
  - [Uniq](#uniq)
  - [Head and Tail](#head-and-tail)
  - [Grep](#grep)
  - [Wc](#wc)
  - [Tr](#tr)
  
- [Unicode](#unicode)
  - [Convert to Ascii](#convert-to-ascii)
  - [Convert to each Other](#convert-to-each-other)
- [ulimit](#ulimit)


[top](#top)
# Enviroments  
### PATH
you know it.
### CDPATH
like [PATH](#path) just for _`cd`_ command.
```vim
  CDPATH=/etc:/var:...
```
### mkdir and cd to that
use below code in your bash file
```vim
  mkcd() { mkdir -p "$@" && cd "$@" }
```

[top](#top)
# Jobs
- \& :
- fg :
- bg :
- disown :
  `run command`,<kbd>Ctrl</kbd><kbd>Z</kbd>,`disown -h Job-ID`

[top](#top)
# Users
* ### Change username:
 - becare first find OldName user and group ID.
```vim
  usermod -l NewName OldName
  groupmod -n NewName oldName
  usermod -d /home/NewName -m NewName
  find / -group OldGroupID -exec chgrp -h NewName {} \;
  find / -user OldUserID -exec chown -h Newname {} \;
```
* ### useradd
```vim
  -e Account Expir Date
  -f Password Expire
  -M without Home Directory
  -d Specified Home Directory
  -N without Group
  -s specified shell
  -L without loggin permission
```
* ### usermod
- Change Primary Group:
```vim
  usermod -g GroupName UserName
```
- Change UserId:
```vim
  usermod -u UserId UserName
```

[top](#top)

# IPv6
* ### disabling IPv6
```vim
  vim /etc/sysctl.conf
  net.ipv6.conf.all.disable_ipv6 = 1
  net.ipv6.conf.default.disable_ipv6 = 1
  sysctl -p
```

[top](#top)

# VPN Routes
* ### [coonect client to server which will connected to vpn:](http://unix.stackexchange.com/questions/237460/ssh-into-a-server-which-is-connected-to-a-vpn-service)
 + Public IP is 50.1.2.3
 +  Public IP Subnet is 50.1.2.0/24
 +  Default Gateway is x.x.x.1
 +  eth0 is device to gateway
```vim
  ip rule add table 128 from 50.1.2.3
  ip route add table 128 to 50.1.2.0/24 dev eth0
  ip route add table 128 default via x.x.x.1
```
* ### Scans
```vim
  IPTABLES -A INPUT -p tcp -i eth0 -m state --state NEW -m recent --set
  IPTABLES -A INPUT -p tcp -i eth0 -m state --state NEW -m recent --update --seconds 30 --hitcount 10 -j DROP
```

[top](#top)

# Partitioning
- [Parted](#parted)
- [dd](#dd)

### Parted
- add partition to existing free space(may raid) :
  1. `# parted` --> so you intered in (parted) mode.
  2. `select /dev/md100` --> inside of disk.
  3. `unit s print free` --> give fisrt and last sector number for free unpatitioned space on disk.
  4. `mkpart`  --> and go ahead and answer questions.
  5. `quit` --> goback to __bash__ mode.
  6. `mkfs. ... NewPartitionName`
[top](#top)
### dd
- create bootable usb:
```vim
  dd if=/Path/to/Image.iso of=/dev/sdx oflag=direct  bs=1048576 && sync
```


[top](#top)
# Files Operating
- [rsync](#rsync)
- [find](#find)

### rsync:
```vim
  rsync -a(~r(recusrsive)l(link)p(permission)o(owner)t(time modify)g(group)D(device,specials) -z(compress) \
  -h(human) -v(verbose) --include --exclude --remove-source-files --delete -u(updated) -d(sync directory) \
  --append(append to shorter files) -L(change symbol link to real) -W(copy file whole) --inplace(directly write) \
  --max-size --bwlimit
```
   
   1.Only Drive your Code:
   ```vim
     rsync --drive-run ...
   ```
   2.Copy/Sync Files and Directory Locally:
   ```vim
     rsync -zhv SourceFILE Destination/
     rsync -azhv SourceDIR Destination/
     rsync -azhv -e 'ssh' SourceDir root@IP:/Path
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
   ```vim
     rsync -azhv --progress SourceDir Destination/
   ```
   4.Include & Exclude:
   ```vim
     rsync -azhv --include '*.R' --exclude '*e*.R' SourceDir/ Destination/
   ```
   5.delete source files or destination files
   ```vim
     rsync -azhv --remove-source-files Source/ Destination/
     rsync -azhv --delete Source/ Destination/
   ```
      1. --delete: if folder exist in dest but not remain in source will be delete from Dest.
      2.you could use `--remove-source-files` instead of `mv` command.
  
   6.files size & Bandwidth limitation: will sync less or equeal than
   ```vim
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
### find
```vim
  find multiple paths -name "Pattern" -type Type  -not -name "Pattern" -exec SomeCommands {} SomeCommands \;
```
 1.find in many Path
 
 2.find with specefic type: f-->file d-->directory
 
 3.find with extention: 
 ```vim
   find ... -type f \( -name "\*.cpp" -o -name "\*.csv" \)
 ```
 4.Sample of commands:
 ```vim
   find . -iname "cpp" -type f -exec cp {} /home/root/Bkp/ \;
   find . -name "sample*.py" -type f -exec grep -B5 -A8 'function' {} \;
 ```
 5.find by midification time:
 ```vim
   find . -mtime 1  <--24 hours
   find . -mtime -5 <-- 5 days ago
 ```
 6.find and tar:
 ```vim
   find . -type f \( -name "*.py" \) print0 | xargs -0 tar cvf myPyFile.tar  <--print0 handle space in file name.
 ```
 7.find not a User as Owner:
 ```vim
   find /Path \! -user UserName
 ```
 
[top](#top)
# Scripting
- [Logging](#logging)
- [Variable](#variable)
- [Input Prameters](#input-parameters)
- [Loop](#loop)
- [Conditional](#conditional)

[top](#top)
### Logging
- generate log of output:
```vim
  exec 3>&1 4>&2
  trap 'exec 2>&4 1>&3' 0 1 2 3
  exec 1>log.out 2>&1
```

[top](#top)
### Varisable
- define variable: `Var=Value` <-- be care _No Space_
- defualt value for variable: `[ "$Var" == "" ] && Var=DefaultValue`
- Variable dealing:
  - assign empty variable: simply use `VariableName=`
  - incrementally assign: `VariableName="$VariableName ..."` <-- be care about <kbd>Space</kbd>


[top](#top)
### Input Parameters
- input Parameter : `Par1=$1` , ... <-- be care _No Space_
- input arguments:
 - `$@` : a list of all input arguments
 - `$#` : number of input arguments


[top](#top)
### Array
- String to array: `IFS='sep' read -ra ListName <<< $1`
- Length of Array: `${#ArrayName[@]}`
  
[top](#top)
### Loop
- Loop:
  - for:
  ```sh
    for 1 in {1..100};do ...;done
    for 1 in {1..100..10};do ...;done
    for ((i=0;i<No.;i++));do ...;done
  ```
[top](#top)
### Conditional
- if : structure is:
```vim
  if [ cluase ] 
  then
    Block
  elif [ Cluase ]
  then
    Block
  else
    Block
  fi
```
  1. Switchs:
  A Taxonomy of Data Science
  
Primary|Meaning
---|---  
[ -a FILE ]	|True if FILE exists.
[ -b FILE ]	|True if FILE exists and is a block-special file.
[ -c FILE ]	|True if FILE exists and is a character-special file.
[ -d FILE ]	|True if FILE exists and is a directory.
[ -e FILE ]	|True if FILE exists.
[ -f FILE ]	|True if FILE exists and is a regular file.
[ -g FILE ]	|True if FILE exists and its SGID bit is set.
[ -h FILE ]	|True if FILE exists and is a symbolic link.
[ -k FILE ]	|True if FILE exists and its sticky bit is set.
[ -p FILE ]	|True if FILE exists and is a named pipe (FIFO).
[ -r FILE ]	|True if FILE exists and is readable.
[ -s FILE ]	|True if FILE exists and has a size greater than zero.
[ -t FD ]	|True if file descriptor FD is open and refers to a terminal.
[ -u FILE ]	|True if FILE exists and its SUID (set user ID) bit is set.
[ -w FILE ]	|True if FILE exists and is writable.
[ -x FILE ]	|True if FILE exists and is executable.
[ -O FILE ]	|True if FILE exists and is owned by the effective user ID.
[ -G FILE ]	|True if FILE exists and is owned by the effective group ID.
[ -L FILE ]	|True if FILE exists and is a symbolic link.
[ -N FILE ]	|True if FILE exists and has been modified since it was last read.
[ -S FILE ]	|True if FILE exists and is a socket.
[ FILE1 -nt FILE2 ]	|True if FILE1 has been changed more recently than FILE2, or if FILE1 exists and FILE2 does not.
[ FILE1 -ot FILE2 ]	|True if FILE1 is older than FILE2, or is FILE2 exists and FILE1 does not.
[ FILE1 -ef FILE2 ]	|True if FILE1 and FILE2 refer to the same device and inode numbers.
[ -o OPTIONNAME ]	|True if shell option "OPTIONNAME" is enabled.
[ -z STRING ]	|True of the length if "STRING" is zero.
[ -n STRING ] or [ STRING ]	|True if the length of "STRING" is non-zero.
[ STRING1 == STRING2 ] |True if the strings are equal. "=" may be used instead of "==" for strict POSIX compliance.
[ STRING1 != STRING2 ] |True if the strings are not equal.
[ STRING1 < STRING2 ] |True if "STRING1" sorts before "STRING2" lexicographically in the current locale.
[ STRING1 > STRING2 ] |True if "STRING1" sorts after "STRING2" lexicographically in the current locale.
[ ARG1 OP ARG2 ] |"OP" is one of -eq, -ne, -lt, -le, -gt or -ge. These arithmetic binary operators return true if "ARG1" is equal to, not equal to, less than, less than or equal to, greater than, or greater than or equal to "ARG2", respectively. "ARG1" and "ARG2" are integers.

[top](#top)

  2. Combining expressions

Operation	|Effect|
---|---
[ ! EXPR ]	|True if EXPR is false.|
[ ( EXPR ) ]	|Returns the value of EXPR. This may be used to override the normal precedence of operators.|
[ EXPR1 -a EXPR2 ]	|True if both EXPR1 and EXPR2 are true.|
[ EXPR1 -o EXPR2 ]	|True if either EXPR1 or EXPR2 is true.|

[top](#top)
# Text Processing
## REGEX
### Regex Table
__First__ [this Site](https://regex101.com/) is very A Taxonomy of Data Sciencegreate for Online REgex Testing.

Character|Meaning|Example
---|---|---
\*|Match zero, one or more of the previous|[Ah\*](#Bash.md) _matches_ "Ahhhhh" or "A"
?|Match zero or one of the previous but Optional|[Ah?](#Bash.md) _matches_ "Al" or "Ah"
\+|Match one or more of the previous |[Ah+](#Bash.md) _matches_ "Ah" or "Ahhh" but not "A"                                       
\\ |Used to escape a special character|[Hungry\\?](#Bash.md) _matches_ "Hungry?"                                                   
\.|Wildcard char,matches any character|[do.\*](#Bash.md) _matches_ "dog", "door", "dot", etc.                                     
( )|Group characters and preserver sequences|(abc) _mathces_ [abc](#Bash.md) asdf bwefd cwef esr[abc](#Bash.md)ftuj.                                                                             
[ ]|Matches a range of characters|[[cbf]ar](#Bash.md) _matches_ "car", "bar", or "far"<br/>[0-9]+ matches any positive integer<br/>[[a-zA-Z]](#Bash.md) _matches_ ascii letters a-z (UP and LO case)<br/>[[^0-9]](#Bash.md) _matches_ any character not 0-9.                                        
\||Matche previous OR next<br/>character/group|[(Mon)\|(Tues)day](#Bash.md) _matches_ "Monday" or "Tuesday"                                
{ }|Matches a specified number of<br/>occurrences|[[0-9]{3}](#Bash.md) _matches_ "315" but not "31"<br/>[[0-9]{2,4}](#Bash.md) _matches_ "12", "123", and "1234"<br/>[[0-9]{2,}](#Bash.md) _matches_ "1234567..."                          
^|Beginning of a string Or within a<br/>character range [] negation|[^http](#Bash.md) _matches_ strings that begin with http,such as a url<br/>[[^0-9]](#Bash.md) _matches_ any character not 0-9.                   
$|End of a string.|[ing$](#Bash.md) _matches_ "exciting" but not "ingenious"
?=|Positive Lookahead<br/>When you look a pattern that followed by some patterns|[(\w\+\|\d\+)his\s(?=Followed)](#Bash.md) _matches_ with [This](#) Followed but This one is not.
?!|Negative Lookahead<br/> Opposite of abive|[(\w\+\|\d\+)his\s(?=Followed)](#Bash.md) _matches_ with This Followed but [This](#) one is not.
?<=|Positive Lookbehind<br/> Like Positive Lookahead but we looking for second pattern|(?<=(\w{1}\|\d{1})his)\s(Followed) _matches_ This [Followed](#Bash.md) but " Followed " word not and this.
?<!|Negative Lookbehind<br/> Like Positive Lookahead but we looking for second pattern|(?<!(\w{1}\|\d{1})his)\s(Followed) _mathches_ This Followed but " [Followed](#Bash.md) " word not and this .


__Shorthand Character Sets__

Shorthand|Description
---|---
.|Any character except new line
\\w|Matches alphanumeric characters: [a-zA-Z0-9_]
\\W|Matches non-alphanumeric characters: [^\w]
\\d|Matches digit: [0-9]
\\D|Matches non-digit: [^\d]
\\s|Matches whitespace character: [\t\n\f\r\p{Z}]
\\S|Matches non-whitespace character: [^\s]

__Chanie Patterns followed__


[top](#top)

## AWK
- Kill Prossecc:
```vim
    kill -9 `ps faux | grep Name | awk -F\  '{print $2}''`
```
- Calculate total ram using with App:
```vim
  ps faux|grep App| awk '{Sum=+$6} END {print Sum/1024/1024 "G"}'
```

## CUT

- return carachters No1 to No2 for each line:
```vim
  cut -cNo1-No2 File
```
- return column No while Seperator is Col seperator:
```vim
  cut -d'Seperator' -fColNo1-ColNo2,ColNo3
```
- show all cols except specified fields:
```vim
  cut -d'Seperator' --complement -s -fNos
```
- Change delemiter out put to _Return_:
```vim
  cut ... --output-delimiter=$'\n'
```

[top](#top)
## SED
- replace a word in file:
```vim
  sed -i 's/OldWord/NewWord/g' FileName
  sed -i.bak ...   <- replace and get backup of original files
```
- replace a word in Files
```vim
  find ./ type f -ecex sed -i 's/OldWord/NewWord/g' {} \;
```
- No.th Line:
```vim
  sed -n 'No.p' or sed 'No.-1q;d' FileName
```
- Multiple Line:
```vala
  sed -n 'No1.,No2p' FileName
```
- Remove (a) Lines:
 ```vim
   sed '/Pattern/d' FileName # Containing a pattern
   sed 'No1..No2d' FileName
 ```
  - MultiLine
   ```vim
     sed '/Pattern/, +5 d' FileName
   ```
[top](#top)
## Sort

[top](#top)
## Uniq

[top](#top)
## Head and Tail
```vim
  head -n No. File
  tail tail -n No. File
```
[top](#top)
## Grep
```vim
  grep -n ... # show line number of match
  grep [-e|-E] "Pattern" ... # Search in file for BRE|ERE pattern
  grep -w "Word" ... # Find whole of word
  grep [-A|-B|-C] No. .... # Print No. Lines After|Before Match|both of before
  grep -c ... # Count of matvh
  grep -r -v ... # recursively search with reverse search
  grep -l ... # display only file name that match
  grep -o ... # showing only matched string
```
  - In basic regular expressions(BRE) the meta-characters ?,+,{,|,(,) lose their special meaning; 
    instead use the backslashed versions \\?,\\+,\\{,\\|,\\(,\\),but not in extended regular experssion(ERE)

[top](#top)
## Wc
```vim
  wc -l File # Number Lines
  wc -w File # NUmber Words
  wc -m File # NUmber Characters
  wc -L File # lengest line width 
```
[top](#top)
## Tr
it is useful to make nice the output of processing:
```vim
  tr -d [:punct:] <inFileName > outFileName
  tr [:upper:] [:lower:] ...
```  

[top](#top)
# Unicode
- [Convert to Ascii](#convert-to-ascii)
- [Convert to each other](#convert-to-each-other)

### Convert to Ascii
this command will convert FIn to FOut from FirstCoding to ascii:
```vim
  native2ascii -encoding FiratCoding -reverse FIn FOut
```
[top](#top)
# ulimit
[this](https://serverfault.com/a/487641/209634)is a good explanation

