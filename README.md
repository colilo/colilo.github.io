# colilo.github.io

## GDB
* https://sourceware.org/gdb/onlinedocs/gdb/TUI-Keys.html#TUI-Keys
* define xxd in gdb
```
define xxd
dump binary memory dump.bin $arg0 $arg0+$arg1
shell xxd dump.bin
end
```

##
