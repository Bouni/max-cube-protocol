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

