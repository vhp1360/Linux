<div dir="rtl">بنام خدا</div>
######## Top

- [Scripting](#scripting)

- [BashProgramming](#bashprogramming)



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
