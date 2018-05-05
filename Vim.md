<div dir="rtl">بنام خدا</div><br/>
- [Text Highlight](text-highlight)
- [Replace](eplace)
- [Macro](macro)
- [Scroll](scroll)
- [Editing](editing)
- [Change Character Case](change-character-case)
- [Run Shell Command](run-shell-command)
- [Multiple File](multiple-file)

### Text Highlight
1- Text Highlight
```vim
  :syntax on
  :set syntax=php
```
2- <kbd>:</kbd><kbd>n</kbd><kbd>o</kbd><kbd>h</kbd> <- remove last search highlight


### Replace : 
  - inline: <kbd>:</kbd><kbd>%</kbd><kbd>s</kbd><kbd>/</kbd>_OldWord_<kbd>/</kbd>_NewWord_
  - globale: <kbd>:</kbd><kbd>%</kbd><kbd>s</kbd><kbd>/</kbd>_OldWord_<kbd>/</kbd>_NewWord_<kbd>/</kbd><kbd>g</kbd>
  - special Lines: <kbd>:</kbd><kbd>_N1_</kbd><kbd>,</kbd><kbd>_N2_</kbd><kbd>%</kbd><kbd>s</kbd><kbd>/</kbd>_OldWord_<kbd>/</kbd>_NewWord_
  - current word: <kbd>*</kbd>

### Macro
  - <kbd>qd</kbd>   <- to start macro
  - ...  <- Doing your commands
  - <kbd>q</kbd>    <- Exit macro
  - <kbd>@</kbd><kbd>d</kbd>   <- to first run
  - <kbd>n</kbd><kbd>@</kbd><kbd>@</kbd>  <- n times repeated

### Scroll
  - <kbd>z</kbd><kbd>z</kbd> - move current line to the middle of the screen 
  - <kbd>z</kbd><kbd>t</kbd> - move current line to the top of the screen 
  - <kbd>z</kbd><kbd>b</kbd> - move current line to the bottom of the screen

### Editing :
  - edit a word : 
    - <kbd>c</kbd><kbd>i</kbd>\{<kbd>w</kbd>|<kbd>[</kbd>|<kbd>(</kbd>|<kbd>{</kbd>} --> change inner word|all between ()|...
    - <kbd>c</kbd><kbd>w</kbd> --> change word from cursor to end
    - <kbd>c</kbd><kbd>a</kbd><kbd>w</kbd> --> change word
  - edit line :
    - <kbd>Shift</kbd><kbd>c</kbd> --> change current line to end
    - <kbd>c</kbd><kbd>c</kbd> --> change whole of current line
### Change Character Case
  - change case of current char : <kbd>~</kbd>
  - change case of current word from cursor : <kbd>g</kbd><kbd>~</kbd><kbd>w</kbd>
  - change case of whole of current word : <kbd>g</kbd><kbd>~</kbd><kbd>i</kbd><kbd>w</kbd>
  - to upper/lower current word : <kbd>g</kbd><kbd>U/u</kbd><kbd>i</kbd><kbd>w</kbd>
  - you can use visualy, <kbd>v</kbd> select word and use <kbd>~</kbd>

### Run Shell Command
- run one command: <kbd>:</kbd><kbd>!</kbd>_Command_
- go in __Shell__ mode: <kbd>:</kbd><kbd>s</kbd><kbd>h</kbd>
- get return back to current opened file by vim: <kbd>:</kbd><kbd>r</kbd><kbd><kbd>!</kbd>Command
  
### Multiple Files



