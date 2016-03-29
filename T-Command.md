## The T Command

The `t:` command deletes one or multiple devices from the Cube. 
The cube  acknowledge the the processing of a command with an `A:` message. 


```
t:01,1,Dx1U
A:
```

Details
```
Position   Information
===================================================
t:        Command
01        number of devices to be deleted
1         Force yes/no
Dx1U      base64 encoded list of Room (rfaddresses)
```
