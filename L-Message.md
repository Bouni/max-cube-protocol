## The L Message

This message is amongst those sent by the cube on connect. Send an 'l' command to get the cube to send it again.
    
    l:\r\n

The L message looks like this:

    L:Cw/a7QkSGBgoAMwACw/DcwkSGBgoAM8ACw/DgAkSGBgoAM4A

The hexdump:

    0000000000:  4c 3a 43 77 2f 61 37 51 6b 53 47 42 67 6f 41 4d  :L:Cw/a7QkSGBgoAM
    0000000010:  77 41 43 77 2f 44 63 77 6b 53 47 42 67 6f 41 4d  :wACw/DcwkSGBgoAM
    0000000020:  38 41 43 77 2f 44 67 41 6b 53 47 42 67 6f 41 4d  :8ACw/DgAkSGBgoAM
    0000000030:  34 41 0D 0A                                      :4A..

Like the M message, the main part is base64 encoded. When decoded, it looks like this:

    0000000000:  0b 0f da ed 09 12 18 18 28 9d 0b 04 0b 0f c3 73  :................
    0000000010:  09 12 18 18 28 00 cf 00 0b 0f c3 80 09 12 18 18  :................
    0000000020:  28 00 ce 00                                      :....

This part of the messsage consists of following fields:

    Offset  Description         Length      Example Value
    ==============================================================================
    0       Submessage Length   1           6
    1-3     RF Address          3           0FDAED
    4       Unknown             1           09
    5-6     Flags               2           1218

If the submessage length is greater than 6, these fields follow:

    Offset  Description         Length      Example Value
    ==============================================================================
    7       Valve Position      1           128
    8       Temperature         1           14
    9-10    Date Until          2           9D0B - Appears not to apply for heater thermostat
    11      Time Until          1           4

The (LSB of byte at offset 9 * 256) plus byte at offset 10 sometimes contains the heater thermostat actual temperature:

    9-10     Actual Temperature  2           205

If the submessage length is 12 (for a WallMountedThermostat), the actual temperature is always provided here:

    Offset  Description         Length      Example Value
    ==============================================================================
    12      Actual Temperature  1           219

A L message can consist of many submessages, but is always terminated by 

    0000000020:        ce 00 

### Submessage Length

    0000000000:  0b
    
This determines how long the following submessage will be. Eco buttons will have 8 byte long submessages, while valves have 11 byte long submessages. A WallMountedThermostat adds another byte, making a 12 byte message.

### RF Address

    0000000000:     0f da ed

These 3 bytes are the RF Address of the device

### Unknown

    0000000000:              09

The meaning of this byte is currently unknown.

### Flags

    0000000000:                 12 18

These two bytes can be decoded as following:

    hex:  |    12     |    18     |
    dual: | 0001 0010 | 0001 1000 |
               | |||    |||| | ||
               | |||    |||| | ++--- mode: 00=auto/weekly program
               | |||    |||| |             01=manual
               | |||    |||| |             10=vacation
               | |||    |||| |             11=boost
               | |||    |||| |
               | |||    |||| +------ dst setting: 0=inactive
               | |||    ||||                      1=active
               | |||    ||||
               | |||    |||+-------- gateway: 0=unknown
               | |||    |||                   1=known
               | |||    |||
               | |||    ||+--------- panel: 0=unlocked
               | |||    ||                  1=locked
               | |||    ||
               | |||    |+---------- link status: 0=ok
               | |||    |                         1=error
               | |||    |
               | |||    +----------- battery: 0=ok
               | |||                          1=low
               | |||
               | ||+---------------- status initialized: 0=not initialized
               | ||                                      1=initialized
               | ||
               | |+----------------- answer: 0=this message is an answer to a command
               | |                           1=this message is not an answer to a command
               | |
               | +------------------ error: 0=no
               |                            1=yes
               |
               +-------------------- valid: 0=information is invalid
                                            1=information provided is valid

### Valve Position

    0000000000:                      18

The valve position indicates the position of the radiator valve. 100 is fully open, 0 is closed.
A WallMountedThermostat always returns '4' for this value.

### Temperature

    0000000000:                          28

The temperature indicates the configured temperature and the mode of a device. It can be decoded as following:

    hex:  |    28     |
    dual: | 0010 1000 |
            | || ||||
            | ++-++++-- temperature: 10 1000 -> 40 = temp * 2
            |                      (to get the actual temperature, the value must be divided by 2: 40/2 = 20)
            |
            |
            +---------- actual temperature MSB

The mode here is not updated; the mode from the second flag byte is correct though.

### Date Until

    0000000000:                             9d 0b

Date until indicates to which date the given temperature is set. It does not appear to apply to the radiator thermostat.  It can be decoded as following:

               +-++++--------------- day: 1 1101 -> 29
               | ||||  
    dual: | 1001 1101 | 0000 1011 | (9D0B)
            |||          | | ||||
            |||          | +-++++--- year: 0 1011 -> 11 = year - 2000
            |||          |                 (to get the actual year, 2000 must be added to the value: 11+2000 = 2011)
            |||          |
            +++----------+---------- month: 1000 -> 8

In this example the temperature is set till Aug 29, 2011.

### Time Until

    0000000000:                                   04

Time until indicates to which date the given temperature is set. In this example it is set to 2:00 (04 * 0,5 hours = 2:00)

### Actual Temperature (HeaterThermostat)

If a HeaterThermostat is in 'auto' mode, the actual temperature is sometimes returned at offset 9-10. This seems to only appear when the valve moves, although changing the set temperature might be enough. In adition, if the HeaterThermostat is in a room that has a linked WallMountedThermostat, this shows the temperature measured by the WallMountedThermostat instead.

    9       Actual Temperature  2           205

    offset|      9    | ... |      10    |
    hex   |     01    |     |     32    |
    binary| 0000 0001 | ... | 0011 0010 |
                    |         |||| ||||
                    +---------++++-++++--- actual temperature (°C*10): 100110010 = 30.6°C
    
### Actual Temperature (WallMountedThermostat)

    11      Actual Temperature  1           219

Room temperature measured by the wall mounted thermostat in °C * 10. For example 0xDB = 219 = 21.9°C
The temperature is represented by 9 bits; the 9th bit is available as the top bit at offset 8

    offset|      8    | ... |     12    |
    hex   |     B2    |     |     24    |
    binary| 1011 0010 | ... | 0010 0100 |
            | || ||||         |||| ||||
            | ++-++++--------------------- temperature (°C*2):            110010 = 25.0°C
            |                 |||| ||||
            +-----------------++++-++++--- actual temperature (°C*10): 100100100 = 29.2°C

### Window switch

The windows status is mapped in the lowest two bits in the flag word. 
Window closed: 00=auto/weekly program, window open: 10=vacation
