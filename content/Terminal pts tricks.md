```
echo "hello" >> /dev/pts/1 
```

*prints “hello” to the terminal 1 

## Why it works 
In unix system every terminal is represented by a special file under /dev/pts. 
So you can just pipe text into it and it will be printed 

![[Pasted image 20260619183219.png]]


