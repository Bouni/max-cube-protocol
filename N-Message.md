## The n Command

The n command sets th Cube into pairing mode.
There are 2 ways to invoke this command

Send `n:` to the Cube, starts pairing mode without timeout. e.g

    n:\r\n

Send `n:` to the Cube, starts pairing mode with timeout (in seconds in HEX)
e.g with timeout of 60 sec (60 sec = 0003c in HEX)

    n:003c\r\n

 
## The N Message 

The N message provides information about newly paired MAX! devices. This response is given after pairing started with the `n:` command.

  `N:Aw4VzExFUTAwMTUzNDD/`
  
  The N Message is a base64 decoded string. After decoding:
  
  `00000000  03 0e 15 cc 4c 45 51 30  30 31 35 33 34 30 ff     |....LEQ0015340.|`

  
      Description     Length      Example Value
    =====================================================================
    DeviceType         1          03 = wallthermostat
    RF address         3          0e15cc
    Serial             10         LEQ0015340
    unknown            1          ff
  
