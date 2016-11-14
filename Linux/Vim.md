<div dir="rtl">بنام خدا</div><br/>
remove last search highlight --> :noh<br/>
search : 

```
         1- inline        --> :%s/OldeWord/NewWord/
         2- globale       --> :%s/OldWord/NewWord/g
         3-special Lines  --> :N1,N2%s/OldWord/NewWord/
         4-
```
Text Highlight

```
         :syntax on
         :set syntax=php
```
* Macro
 - <kbd>qd</kbd>   -> to start macro
 - ...  -> Doing your commands
 - <kbd>q</kbd>    -> Exit macro
 - <kbd>@</kbd><kbd>d</kbd>   -> to first run
 - <kbd>n</kbd><kbd>@</kbd><kbd>@</kbd>  -> n times repeated

* Scroll
 - <kbd>z</kbd><kbd>z</kbd> - move current line to the middle
   of the screen 
 - <kbd>z</kbd><kbd>t</kbd> - move current line
   to the top of the screen 
 - <kbd>z</kbd><kbd>b</kbd> - move
   current line to the bottom of the
   screen
- edit :
  - edit a word : 
    - <kbd>c</kbd><kbd>i</kbd>\{<kbd>w</kbd>|<kbd>[</kbd>|<kbd>(</kbd>|<kbd>{</kbd>} --> change inner word|all between ()|...
    - <kbd>c</kbd><kbd>w</kbd> --> change word from cursor to end
    - <kbd>c</kbd><kbd>a</kbd><kbd>w</kbd> --> change word
  - edit line :
    - <kbd>Shift</kbd><kbd>c</kbd> --> change current line to end
    - <kbd>c</kbd><kbd>c</kbd> --> change whole of current line
  
         
