### native packages

:packadd - start a plugin that was put into "opt" folder (not start and hence not autostarted)

:scriptnames - list all the files that are loaded in by plugins

#### folding

[Vim tips: Folding fun - Linux.com](https://www.linux.com/training-tutorials/vim-tips-folding-fun/)

### quickfix vs location list

* quickfix is global per vim session while location list is specific to a particular window

#### quickfix commands

```
:copen
:cnext  -- customised to C-n
:cprev  -- customised to C-m
:cc
:cf <file>
:cclose
```

#### location list commands

```
:lopen
:lnext
:prev
:ll
:lf <file>
:lclose
```