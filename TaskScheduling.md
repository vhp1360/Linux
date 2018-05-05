<div dir=rtl>بنام خدا</div>

- [Cron](#cron)
- [CronTab](#crontab)
- [At](#at)

## Cron



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
