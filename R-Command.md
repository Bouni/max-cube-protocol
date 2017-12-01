
## The r Command

The `r:` command sends a reset command to a device. 
The cube acknowledges the the processing of the command with an `S:` message. 

I found the command while the cube reported an error for this device. I tried to send an `s:` command to a device with an error. 
The cubes software first sends the `r:` command and after getting the `S:` answer the the initial `s:` command was sent.

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

