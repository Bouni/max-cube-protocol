# eQ3/ELV MAX! Cube Protocol

## Disclaimer

The main info about the eQ3 Cube Protocol comes from these sources:

- http://www.domoticaforum.eu/viewtopic.php?f=66&t=6654
- https://github.com/bietiekay/hacs/tree/master/tools/MAXDebug
- decompiling the Original MAX Software from eQ3/ELV

But i have seen that some of the information that is provided there is not correct (at least for my cube which runs firmware verison 1.1.3). For that reason i've started to describe the protocol here, so that everybody can contibute and profit.

Feel free to send me pull request or open an issue to help me getting this right!

All information in this file are valid (or at least work for me) with cube version 1.1.3.

## Finding a cube without knowing its IP address

The cube provides the possibility to send a broadcast UDP packet to the network and as soon as it receives it, it answers and we know its IP from that packet.

This is the data (19 Byte) one must broadcast or multicast to UDP port 23272 to get an answer from the cube.

    0000000000: 65 51 33 4D 61 78 2A 00  2A 2A 2A 2A 2A 2A 2A 2A  eQ3Max*.********
    0000000010: 2A 2A 49                                          **I

To find a specific cube on the network replace the `*` (2A) with the serial. Hence to find a specific cube send a discovery packet with the content: `"eQ3Max*\0" + serialnumber + "I"`

The answer (26Byte) can be received on UDP Port 23272 and looks like this:

    0000000000: 65 51 33 4D 61 78 41 70  4B 45 51 30 35 32 33 38  eQ3MaxApKEQ05238
    0000000010: 36 34 3E 49 00 09 7F 2C  01 13                    64>I...,..

It consists of the following parts:

    Description        Length      Example Value
    =====================================================================
    Name?              8           eQ3MaxAp
    Serial number      10          KEQ0523864
    unknown            3           >I
    RF address         3           097F2C
    Firmware version   2           1.1.3          

The most important info of this reply is the IP address that coumes with the UDP packet.
This is a wireshark dump of the received packet:

    No.     Time           Source                Destination           Protocol Length Info
        679 228.941379000  192.168.178.22        192.168.178.23        UDP      68     Source port: s102  Destination port: s102

    Frame 679: 68 bytes on wire (544 bits), 68 bytes captured (544 bits) on interface 0
    Ethernet II, Src: Eq-3Entw_03:81:6c (00:1a:22:03:81:6c), Dst: IntelCor_52:e2:50 (24:77:03:52:e2:50)
    Internet Protocol Version 4, Src: 192.168.178.22 (192.168.178.22), Dst: 192.168.178.23 (192.168.178.23)
    User Datagram Protocol, Src Port: s102 (23272), Dst Port: s102 (23272)
    Data (26 bytes)


You can clearly see the source  IP 192.168.178.22

## Connecting to the cube

You can connect to the cube by establishing a TCP connection with the cube's IP address on port 62910.
To do so you can use telnet for a first test:

    telnet 192.168.178.22 62910

The cube will immediately reply with a lot of information. The further sections will describe what this data means.

## Message structure

A message always (as far as i know) begins with a single character followed by a colon, `H:` for example.
Furthermore a message is always ended by a CR+LF `\r\n`. In between these two marks is the content of the message.

## Message types

There are a lot of message types. I have not seen a lot of them yet, but the list is out of the decompiled source code so it seems to be valid.

### Incoming messages

    INCOMING HELLO:                       "H:"
    INCOMING NTP SERVER:                  "F:"
    INCOMING DEVICE LIST:                 "L:"
    INCOMING CONFIGURATION:               "C:"
    INCOMING METADATA:                    "M:"
    INCOMING NEW DEVICE:                  "N:"
    INCOMING ACKNOWLEDGE:                 "A:"
    INCOMING ENCRYPTION:                  "E:"
    INCOMING DECRYPTION:                  "D:"
    INCOMING SET CREDENTIALS:             "b:"
    INCOMING GET CREDENTIALS:             "g:"
    INCOMING SET REMOTEACCESS:            "j:"
    INCOMING SET USER DATA:               "p:"
    INCOMING GET USER DATA:               "o:"
    INCOMING CHECK PRODUCT ACTIVATION:    "v:"
    INCOMING ACTIVATE PRODUCT:            "w:"
    INCOMING SEND DEVICE CMD:             "S:"

### Outgoing messages

    OUTGOING URL:                         "u:"
    OUTGOING INTERVAL:                    "i:"
    OUTGOING SEND:                        "s:"
    OUTGOING METADATA:                    "m:"
    OUTGOING INCLUSION MODE:              "n:"
    OUTGOING CANCEL INCLUSION MODE:       "x:"
    OUTGOING MORE DATA:                   "g:"
    OUTGOING QUIT:                        "q:"
    OUTGOING ENCRYPTION:                  "e:"
    OUTGOING DECRYPTION:                  "d:"
    OUTGOING SET CREDENTIALS:             "B:"
    OUTGOING GET CREDENTIALS:             "G:"
    OUTGOING SET REMOTEACCESS:            "J:"
    OUTGOING SET USER DATA:               "P:"
    OUTGOING GET USER DATA:               "O:"
    OUTGOING CHECK PRODUCT ACTIVATION:    "V:"
    OUTGOING ACTIVATE PRODUCT:            "W:"
    OUTGOING SEND DEVICE CMD:             "s:"
    OUTGOING RESET:                       "a:"
    OUTGOING RESET ERROR:                 "r:"
    OUTGOING DELETE DEVICES:              "t:"
    OUTGOING SET PUSHBUTTON CONFIG:       "w:"
    OUTGOING GET DEVICE LIST:             "l:"
    OUTGOING SET URL:                     "u:"
    OUTGOING GET CONFIGURATION:           "c:"
    OUTGOING TIME CONFIG:                 "v:"
    OUTGOING NTP SERVER:                  "f:"
    OUTGOING SEND WAKEUP:                 "z:"


## The H Message

The H message looks like this:

    H:KEQ0523864,097f2c,0113,00000000,477719c0,00,32,0d0c09,1404,03,0000\r\n

A hexdump of this string:

    0000000000: 48 3A 4B 45 51 30 35 32  33 38 36 34 2C 30 39 37  H:KEQ0523864,097
    0000000010: 66 32 63 2C 30 31 31 33  2C 30 30 30 30 30 30 30  f2c,0113,0000000
    0000000020: 30 2C 36 64 65 38 62 64  31 32 2C 34 37 37 37 31  0,6de8bd12,47771
    0000000030: 39 63 30 2C 30 30 2C 33  32 2C 30 64 30 63 30 39  9c0,00,32,0d0c09
    0000000040: 2C 31 34 30 34 2C 30 33  2C 30 30 30 30 0D 0A     ,1404,03,0000..

This is the simplest of all messages because most of the data is plain text and it is comma seperated.
As a first step we can clip the first two and the last two characters. 


    Description        Length      Example Value
    =====================================================================
    Serial number      10          KEQ0523864
    RF address         6           097F2C
    Firmware version   4           1.1.3
    unknown            8           00000000
    HTTP connection id 8           477719c0
    Duty cycle         2           00
    Free Memory Slots  2           50
    Cube date          6           2013-12-09
    Cube time          4           20:04
    State Cube Time    2           03
    NTP Counter        4           0000

### Serial number

The serial number is simply a string representing the serial number of the cube.

### RF address

The RF address of the cube as hexstring.

### Firmware version

The firmware verion of the cube as a hex number. `0113` is for cube frimware version `1.1.3`

### unknown

This is a not yet known part of the H message. It seems to be always 00000000.

### HTTP connection id

This is not sure (means i was not able to verify this by reviewing the decompiled source of the MAX Software)

### Duty Cycle

The duty cycle as a hex number.

### Free Memory Slots

Free memory Slots as a hex number.

### Cube Date

The time is a hexstring representation of the cube date YYMMDD, the year is `0d` which is decimal `13`, add 2000 and you get `2013`,
the month `0c` is decimal `12` which means december and the day is `09` which is decimal `9`.

### Cube Time

As the cube date, the time is also a hexstring representation of HH:MM, Hex `14` is decimal `20` and hex `04` is decimal `04`.


## The M Message

The M message looks like this:

    M:00,01,VgIEAQNCYWQK7WkCBEJ1cm8K8wADCldvaG56aW1tZXIK8wwEDFNjaGxhZnppbW1lcgr
    1QAUCCu1pS0VRMDM3ODA0MAZIVCBCYWQBAgrzAEtFUTAzNzk1NDQHSFQgQnVybwICCvMMS0VRMD
    M3OTU1NhlIVCBXb2huemltbWVyIEJhbGtvbnNlaXRlAwIK83lLRVEwMzc5NjY1GkhUIFdvaG56a
    W1tZXIgRmVuc3RlcnNlaXRlAwIK9UBLRVEwMzgwMTIwD0hUIFNjaGxhZnppbW1lcgQB\r\n

Again a hexdump:

    0000000000: 4D 3A 30 30 2C 30 31 2C  56 67 49 45 41 51 4E 43  M:00,01,VgIEAQNC
    0000000010: 59 57 51 4B 37 57 6B 43  42 45 4A 31 63 6D 38 4B  YWQK7WkCBEJ1cm8K
    0000000020: 38 77 41 44 43 6C 64 76  61 47 35 36 61 57 31 74  8wADCldvaG56aW1t
    0000000030: 5A 58 49 4B 38 77 77 45  44 46 4E 6A 61 47 78 68  ZXIK8wwEDFNjaGxh
    0000000040: 5A 6E 70 70 62 57 31 6C  63 67 72 31 51 41 55 43  ZnppbW1lcgr1QAUC
    0000000050: 43 75 31 70 53 30 56 52  4D 44 4D 33 4F 44 41 30  Cu1pS0VRMDM3ODA0
    0000000060: 4D 41 5A 49 56 43 42 43  59 57 51 42 41 67 72 7A  MAZIVCBCYWQBAgrz
    0000000070: 41 45 74 46 55 54 41 7A  4E 7A 6B 31 4E 44 51 48  AEtFUTAzNzk1NDQH
    0000000080: 53 46 51 67 51 6E 56 79  62 77 49 43 43 76 4D 4D  SFQgQnVybwICCvMM
    0000000090: 53 30 56 52 4D 44 4D 33  4F 54 55 31 4E 68 6C 49  S0VRMDM3OTU1NhlI
    00000000A0: 56 43 42 58 62 32 68 75  65 6D 6C 74 62 57 56 79  VCBXb2huemltbWVy
    00000000B0: 49 45 4A 68 62 47 74 76  62 6E 4E 6C 61 58 52 6C  IEJhbGtvbnNlaXRl
    00000000C0: 41 77 49 4B 38 33 6C 4C  52 56 45 77 4D 7A 63 35  AwIK83lLRVEwMzc5
    00000000D0: 4E 6A 59 31 47 6B 68 55  49 46 64 76 61 47 35 36  NjY1GkhUIFdvaG56
    00000000E0: 61 57 31 74 5A 58 49 67  52 6D 56 75 63 33 52 6C  aW1tZXIgRmVuc3Rl
    00000000F0: 63 6E 4E 6C 61 58 52 6C  41 77 49 4B 39 55 42 4C  cnNlaXRlAwIK9UBL
    0000000100: 52 56 45 77 4D 7A 67 77  4D 54 49 77 44 30 68 55  RVEwMzgwMTIwD0hU
    0000000110: 49 46 4E 6A 61 47 78 68  5A 6E 70 70 62 57 31 6C  IFNjaGxhZnppbW1l
    0000000120: 63 67 51 42 0D 0A                                 cgQB..

Here is the structure less clear than in an H Message. First we need to clip the first and the last two bytes (removing the M: and CR+LF) 
The Message is comma seperated into 3 parts.


    Description        Length      Example Value
    =====================================================================
    Index              2           00
    Count              2           01
    Data               variable    base64 encoded

### Index

The `00` is the index, at the moment i can not say what it is used for.

### Count

The second part, `01` is the count. again i cannot say what it is used for.
 
### Data

The data string is base64 encoded and contains the real data of this package. If we decode this string we get this data:

    0000000000: 56 02 04 01 03 42 61 64  0A ED 69 02 04 42 75 72  V....Bad..i..Bur
    0000000010: 6F 0A F3 00 03 0A 57 6F  68 6E 7A 69 6D 6D 65 72  o.....Wohnzimmer
    0000000020: 0A F3 0C 04 0C 53 63 68  6C 61 66 7A 69 6D 6D 65  .....Schlafzimme
    0000000030: 72 0A F5 40 05 02 0A ED  69 4B 45 51 30 33 37 38  r..@....iKEQ0378
    0000000040: 30 34 30 06 48 54 20 42  61 64 01 02 0A F3 00 4B  040.HT Bad.....K
    0000000050: 45 51 30 33 37 39 35 34  34 07 48 54 20 42 75 72  EQ0379544.HT Bur
    0000000060: 6F 02 02 0A F3 0C 4B 45  51 30 33 37 39 35 35 36  o.....KEQ0379556
    0000000070: 19 48 54 20 57 6F 68 6E  7A 69 6D 6D 65 72 20 42  .HT Wohnzimmer B
    0000000080: 61 6C 6B 6F 6E 73 65 69  74 65 03 02 0A F3 79 4B  alkonseite....yK
    0000000090: 45 51 30 33 37 39 36 36  35 1A 48 54 20 57 6F 68  EQ0379665.HT Woh
    00000000A0: 6E 7A 69 6D 6D 65 72 20  46 65 6E 73 74 65 72 73  nzimmer Fensters
    00000000B0: 65 69 74 65 03 02 0A F5  40 4B 45 51 30 33 38 30  eite....@KEQ0380
    00000000C0: 31 32 30 0F 48 54 20 53  63 68 6C 61 66 7A 69 6D  120.HT Schlafzim
    00000000D0: 6D 65 72 04 01                                    mer..

The message contains metadata about the rooms and the devices in this rooms.

### Rooms

The first part of this block of data contains the room data for all configured rooms.
The room data is of variable length.
 
#### unknown

    0000000000: 56 02                                             V.

At the moment i cannot say what these two bytes are.


#### Room count

    0000000000:       04                                            .

The room count says how many rooms are configured.

#### Room data
    
    0000000000:          01 03 42 61 64  0A ED 69                    ..Bad..i

On the room count follow the room data for as many rooms as the room count is, here `4` times.
The meaning of the room data is the following:

    Description        Length      Example Value
    =====================================================================
    Roomid             1           1
    Roomname length    1           3
    Roomname           variable    Bad
    Group RF Address   3           0AED69    

#### Room id

    0000000000:          01                                          .

This is the configured room ID.

#### Roomname length

    0000000000:             03                                        .

This tells us how much of the following bytes are the roomname.

#### Roomname

    0000000000:                42 61 64                                Bad

The roomname is as many bytes long as the rommname length.
A roomname length of `03` means one must read the next 3 bytes `42 61 64` which is `Bad` in ASCII.

#### Group RF Adress

    0000000000:                          0A ED 69                         ..i

These 3 bytes are the Group RF Address of the first device in that room.

### Devices
    
This is wat is left after decoding the rooms:

    0000000030:             05 02 0A ED  69 4B 45 51 30 33 37 38      ....iKEQ0378
    0000000040: 30 34 30 06 48 54 20 42  61 64 01 02 0A F3 00 4B  040.HT Bad.....K
    0000000050: 45 51 30 33 37 39 35 34  34 07 48 54 20 42 75 72  EQ0379544.HT Bur
    0000000060: 6F 02 02 0A F3 0C 4B 45  51 30 33 37 39 35 35 36  o.....KEQ0379556
    0000000070: 19 48 54 20 57 6F 68 6E  7A 69 6D 6D 65 72 20 42  .HT Wohnzimmer B
    0000000080: 61 6C 6B 6F 6E 73 65 69  74 65 03 02 0A F3 79 4B  alkonseite....yK
    0000000090: 45 51 30 33 37 39 36 36  35 1A 48 54 20 57 6F 68  EQ0379665.HT Woh
    00000000A0: 6E 7A 69 6D 6D 65 72 20  46 65 6E 73 74 65 72 73  nzimmer Fensters
    00000000B0: 65 69 74 65 03 02 0A F5  40 4B 45 51 30 33 38 30  eite....@KEQ0380
    00000000C0: 31 32 30 0F 48 54 20 53  63 68 6C 61 66 7A 69 6D  120.HT Schlafzim
    00000000D0: 6D 65 72 04 01                                    mer..

#### Device count
 
The device count is, like the room count, the indicator of how many devices are registered.

    0000000030:             05                                        .

#### Device data

    0000000030:                02 0A ED  69 4B 45 51 30 33 37 38       ...iKEQ0378
    0000000040: 30 34 30 06 48 54 20 42  61 64 01 02 0A F3 00 4B  040.HT Bad.....K

The meaning of the room data is the following:

    Description        Length      Example Value
    =====================================================================
    Devicetype         1           2
    RF Adress          3           0AED69
    Serialnumber       10          KEQ0378040
    Device name length 1           6
    Device name        variable    HT Bad
    Room id            1           1

#### Device type
    
    0000000030:                02                                      .

The devicetype indicates what type of device it is:

    0       Cube
    1       Heating Thermostat
    2       Heating Thermostat Plus
    3       Wall mounted Thermostat
    4       Shutter contact
    5       Push Button   

#### RF Address

    0000000030:                   0A ED  69                             ..i

These 3 bytes are the RF Address of the device, here it is `0AED69`.
 
#### Serialnumber

    0000000030:                             4B 45 51 30 33 37 38           KEQ0378
    0000000040: 30 34 30                                          040

The devices serial number `KEQ0378040`

#### Devie name length

    0000000040:          06                                          .

The number of bytes that contain the room name directly after this byte.

#### Device name

    0000000040:             48 54 20 42  61 64                        HT Bad

The device name, `HT Bad` in this case.


### Room id

    0000000040:                                01                           .

The id of the room that contains this device.    



