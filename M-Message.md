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

#### Group RF Address

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
    RF Address         3           0AED69
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

