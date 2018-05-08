<div dir=rtl>بنام خدا</div>

- [Yum](#yum)




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

