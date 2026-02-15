-rw-r--r-- 

[0] - file type 
- `-` = regular file
- `d` = directory
- `l` = symbolic link
- `c` = character device
- `b` = block device
[1-3] - owner  
- `r` = read ✅
- `w` = write ✅
- `x` = execute ✅
[4-6] - group 
- `r` = read ✅
- `-` = write ❌
- `-` = execute ❌
[7-9] - others 



- read = 4
- write = 2
- execute = 1

owner: rw- = 4 + 2 + 0 = 6  
group: r-- = 4 + 0 + 0 = 4  
others: r-- = 4 + 0 + 0 = 4

```sh
chmod 644 filename
```
