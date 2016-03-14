
## Disclaimer
Info from this page mainly comes from http://www.adreamerslair.nl/2014/02/eq-3-max-cube-message-protocol-decrypted-part-4-the-c-word/

## The C Message
The C message looks like this:

    C:03f25d,7QPyXQATAQBKRVEwNTQ0OTIzAAsABEAAAAAAAAAAAPIA==\r\n

The first field contains the address of the device in HEX form. 

The second and last field contains a Base64 encoded string with the configuration data for the device (as identified by its address). The decoded bytes will look something like this for the Cube device itself

## Cube C Message
```
0000: ed 03 f2 5d 00 13 01 00   ........
0008: 4a 45 51 30 35 34 34 39   JEQ05449
0010: 32 33 00 0b 00 04 40 00   23......
0018: 00 00 00 00 00 00 00 ff   ........
0020: ff ff ff ff ff ff ff ff   ........
0028: ff ff ff ff ff ff ff ff   ........
0030: ff ff ff ff 0b 00 04 40   ........
0038: 00 00 00 00 00 00 00 41   .......A
0040: ff ff ff ff ff ff ff ff   ........
0048: ff ff ff ff ff ff ff ff   ........
0050: ff ff ff ff ff 68 74 74   .....htt
0058: 70 3a 2f 2f 6d 61 78 2e   p://max.
0060: 65 71 2d 33 2e 64 65 3a   eq-3.de:
0068: 38 30 2f 63 75 62 65 00   80/cube.
0070: 30 2f 6c 6f 6f 6b 75 70   0/lookup
0078: 00 00 00 00 00 00 00 00   ........
0080: 00 00 00 00 00 00 00 00   ........
0088: 00 00 00 00 00 00 00 00   ........
0090: 00 00 00 00 00 00 00 00   ........
0098: 00 00 00 00 00 00 00 00   ........
00a0: 00 00 00 00 00 00 00 00   ........
00a8: 00 00 00 00 00 00 00 00   ........
00b0: 00 00 00 00 00 00 00 00   ........
00b8: 00 00 00 00 00 00 00 00   ........
00c0: 00 00 00 00 00 00 00 00   ........
00c8: 00 00 00 00 00 00 00 00   ........
00d0: 00 00 00 00 00 00 43 45   ......CE
00d8: 54 00 00 0a 00 03 00 00   T.......
00e0: 0e 10 43 45 53 54 00 03   ..CEST..
00e8: 00 02 00 00 1c 20         ......
```

```
Position   Length   Information
===================================================
0012       1        Is Portal Enabled
0013-0033  32       Pushbutton Up config
0034-0054  32       Pushbutton down config
0055-00D4  127      Portal URL
00D5-00DA  5        TimeZone (Winter)
00DB-00DB  1        TimeZone (Winter) - Month
00DC-00DC  1        TimeZone (Winter) - Weekday
00DD-00DD  1        TimeZone (Winter) - Hour
00DE-00E1  4        TimeZone (Winter) - offset to UTC
00E2-00E6  5        TimeZone (Daylight Savings)
00E7-00E7  1        TimeZone (Daylight Savings) - Month
00E8-00E8  1        TimeZone (Daylight Savings) - Weekday
00E9-00E9  1        TimeZone (Daylight Savings) - Hour
00EA-00ED  4        TimeZone (Daylight Savings) - offset to UTC

```
## Heating thermostat C Message

The first 18 bytes seem to contain the same type of information for all devices (except the Cube which seems to have one difference). The rest of the information is device specific.

So the first 18 bytes contain the following information

```
Position   Length   Information
===================================================
00         1        Data Length
01         3        Address of device
04         1        Device Type
05         1        Room ID
06         1        Firmware version
07         1        Test Result
08         10       Serial Number
```

The device specific bytes contain the following information
```
Pos  Len  Information
================================================================
12   1    Comfort Temperature       in degrees celsius * 2
13   1    Eco Temperature           in degrees celsius * 2
14   1    Max Set Point Temperature in degrees celsius * 2
15   1    Min Set Point Temperature in degrees celsius * 2
16   1    Temperature offset        in degrees celsius * 2 + 3,5
17   1    Window Open Temperature   in degrees celsius * 2
18   1    Window Open Duration      in minutes * 5
19   1    Boost                     3 MSB bits are duration:
                                      value is in minutes * 5
                                      but value 7 means 60 min.
                                    5 LSB bits are valve opening
                                      in % * 5
1a   1    Decalcification           3 MSB bits are day of week:
                                      Saturday = 0 etc.
                                    5 LSB bits are time in hours
1b   1    Max Valve Setting         in % * 255 / 100  (so to get
                                      valve setting use
                                      value * 100 / 255)
1c   1    Valve Offset              in % * 255 / 100
1d   182  Weekly Program            Schedule of 26 bytes for
                                    each day starting with
                                    Saturday. Each schedule
                                    consists of 13 words
                                    (2 bytes) e.g. set points.
                                    1 set point consist of
                                    7 MSB bits is temperature
                                      set point (in degrees * 2)
                                    9 LSB bits is until time
                                      (in minutes * 5)
```

For the above example data this translates to
```
               Raw     Decoded
================================================================
Comfort Temp    24     18 degrees Celsius
Eco Temp        20     16 degrees Celsius
Max Temp        3d     30,5 degrees Celsius
Min Temp        09     4,5 degrees Celsius
Temp Offset     07     0 degrees Celsius
Win Open Temp   18     12 degrees Celsius
Win Open Dur    03     15 minutes
Boost           f4     111 10100 -> 60 minutes, 100 %
Decal           0c     000 01100 -> Saturday, 12:00 hours
Max Valve       ff     100 %
Valve offset    00     0 %
Weekly program  41 20  0100000 100100000
                       -> 16 degrees, until 24:00 
                       etc.
```
                
## Wall Thermostat
```
===============================================================
 
12   1    Comfort Temperature       in degrees celsius * 2
13   1    Eco Temperature           in degrees celsius * 2
14   1    Max Set Point Temperature in degrees celsius * 2
15   1    Min Set Point Temperature in degrees celsius * 2
1d   182  Weekly Program            Schedule of 26 bytes for
                                    each day starting with
                                    Saturday. Each schedule
                                    consists of 13 words
                                    (2 bytes) e.g. set points.
                                    1 set point consist of
                                    7 MSB bits is temperature
                                      set point (in degrees * 2)
                                    9 LSB bits is until time
                                      (in minutes * 5)
cc   3    Unknown
```
For the above example data this translates to
```
               Raw     Decoded
================================================================
Comfort Temp    24     18 degrees Celsius
Eco Temp        20     16 degrees Celsius
Max Temp        3d     30,5 degrees Celsius
Min Temp        09     4,5 degrees Celsius
Weekly program  41 20  0100000 100100000
                       -> 16 degrees, until 24:00
                etc.
```          
