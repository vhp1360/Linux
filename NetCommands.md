<div dir=rtl>بنام خدا</div>

- [Yum](#yum)
- [NetWork](#network)
  - [ncat](#ncat)
  - [wget](#wget)
  - [tcpflow](#tcpflow)


[top](#top)
# Yum
- installing: 
  1. by Package[s] Name[s] -> `yum install PackageName1 PackageName2 ...`
  2. by Group Package Name -> `yum groupinstall "GroupName"`
- updatig: `yum update ...`
- upgrading: `yum upgrade ...`
- removeing: `yum remove ...`
- list Packages: 
  1. for all ->`yum list installed` 
  2. for group `yum grouplist GroupName` 
  3. for single `yum list PackageName`
- search for Package Name: `yum search Name`
- serach which Package Provide specific Command/file/...: `yum provides Name`
- List repositories: `yum repolist all`
- enable and disable a repo: `yum install ... -enablerepo=RepoName`
- clean Cache: `yum clean cache`
- history: `yum history`

[top](#top)
# NetWork
- ###### ncat
  - to send data on machine port and listening.
    1. listening: `nc -l PortNo.`
    2. sending: `cat File | nc IP PortNo.`
- ### wget
  - to download
    1. simple code: `wget http:/.... or ftp://... `
    2. Username&Pass: 
    ```vim
      wget http://userName:Password@Address
      wget --http-user=User --http-password=Password http://...
      wget --ftp-user=User --ftp-password=Password ftp://...
    ```
    3. Recursively of Web: `wget -r http://... or ftp://... `
    4. Resumable : `wget -c http://... or ftp://... `
    5. From Files: `wget -i /Path to Text File `
- ### tcpflow
  - Listning on busy port:
  ```vim
    tcpflow   -i Interface -C[Print On Console] -J[Show Coloring] port PortNo
  ```

[top](#top)
