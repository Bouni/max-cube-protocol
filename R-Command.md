
## The r Command

The `r:` command sends a reset command to a device. 
The cube acknowledges the the processing of the command with an `S:` message. 

I tried to send an `s:` command to a device which has reported an error. The cubes client-software first removes the error with an r:` command.
The cube answered with an `S:` message. After that my initial `s:` command was sent and had been answered by an `S:` message too.


```
r:01,GPbm
S:0c,0,2f
```

Details
```
Position   Information
===================================================
r:        Command
01        I'm not shure but I think its the Device-Typ
GPbm      The base64 encoded RF Address
```

