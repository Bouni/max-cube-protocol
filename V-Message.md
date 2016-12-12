## The v Command

The `v:` command sets the date, time and timezone information for the cube. The cube replies with "A:" after setting the data.


```
v:Q0VUAAAKAAMAAA4QQ0VTVAADAAIAABwg,1fdda096\r\n
A:
```

Details
```
Position Length  Information
===================================================
00          2    "v:" Command
02-21      32    Timezone, 2 lines, Base64 encoded
                 1st line: Winter TZ, eg "CET"
                 2nd line: Summer TZ, eg "CEST"
22          1    Seperator, ","
23-2A       8    Current time (UTC), in seconds
                 since 2000-01-01 00:00 encoded
                 as HEX
2B-2C       2    "\r\n"                 
```

