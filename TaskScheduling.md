<div dir=rtl>بنام خدا</div>

- [Cron](#cron)
- [CronTab](#crontab)
- [At](#at)

## Cron
- to list existing jobs and edit cron:
```vim
  cron -l -u UserName
  cron -e -u UserName
```
- kind of time we could use:<br/>
  1- _*/5_ --> means every 5 period even minutes,hours,...<br/>
  2- _5,10,30_ --> means every 5,10,30 period<br/>
  3- _1-3_ --> means every 1,2,3 of period<br/>
  4- _@reboot_ --> means run once after reboot<br/>
  5- _@yearly_ --> means run once year<br/>
  6- _@monthly_ --> means run once week<br/>
  7- _@daily_ --> means run once day<br/>
  8- _@midnight_ --> means run ever midnight<br/>
  9- _@hourly_ --> means every hour<br/>
  


[top](#top)
## CronTab



[top](#top)
## At
###### how to set:
```vim
  at Time
```
  #### Kind of exist Time :
  - `19:00`
  - `08:00 AM`
  - `19:00 Sun`
  - `19:00 July 20`
  - `19:00 19/08/2018`
  - `19:00 next month`  <-- current date in next month
  - `19:00 tommorow`
  - `now +1 {minute|hour|week|month|year}`
  - `now +2 {minutes|hours|weeks|months|years}`
  - `midnight|noon|teatime`
###### List of Scheduled Task:
```vim
  atq
```
##### Check the Content of Scheduled Task
```vim
at -c TaskNo
```
##### Remove Scheduled Task:
```vim
  atrm TaskNo
```
  


[top](#top)
