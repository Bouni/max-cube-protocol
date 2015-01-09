
The s command is to send settings to the connected devices.
The S message contains the response.

## The S Message
 
The S message looks like this:

    s:AARAAAAAB5EAAWY=\r\n

Like other messages the parameter is base64 encoded.

Converted to hex, this is: 

    0000000000: 00 04 40 00 00 00 07 91  00 01 66 

The first part of the s command  consists of following fields:

    Description        Length      Example Value
    =====================================================================
    Base String        6           000440000000
    RF Address         3           0FDAED
    Room nr            1           01

### Base String

The base string is preceding the detailed settings and determines what parameter will be set.
	
	* 000440000000	Base string for temperature and mode setting
	* 000410000000  Base string for program data setting
	* 000011000000  Base string for eco mode temperature setting

	* 000412000000  Base string for ConfigValveFunctions
	* 000020000000  Base string for AddLinkPartner
	* 000021000000  Base string for RemoveLinkPartner
	* 000022000000  Base string for SetGroupAddress
	* 00  23000000  Base string for RemoveGroupAddress

	Note other commands exist

## the s Temperature and Mode setting command

    Description        Length      Example Value
    =====================================================================
    Base String        6           000440000000
    RF Address         3           0FDAED
    Room nr            1           01
    Temperature  &     1           66
	Mode (similar to the L message)
	Date Until		   1           04  Note: Only in case of vacation mode setting. Otherwise this byte is omitted

	
	
### Temperature & mode

    01100110:                          66

The temperature indicates the configured temperature and the mode of a device. It can be decoded as following:

    hex:  |    66     |
    dual: | 0110 1100 |
            |||| ||||
            ||++-++++-- temperature: 10 1100 -> 38 = temp * 2
            ||                     (to get the temperature, the value must be divided by 2: 38/2 = 19)
            ||
            ||
            ++--------- mode: 00=auto/weekly program
                        01=manual
                        10=vacation
                        11=boost
					
In case of a vacation program
### Date Until

    0000000000:                             9d 0b

Date until indicates to which date the given temperature is set. It can be decoded as following:

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


Time until indicates to which date the given temperature is set. In this example it is set to 2:00 (04 * 0,5 hours = 2:00)

## s command program setting

	s:AAQQAAAACMNJBAJEUVRhRLRVA0UgRSBFIA==

  
	00 04 10 00 00 00 08 c3  49 04 02 44 51 54 61 44  |........I..DQTaD|
	b4 55 03 45 20 45 20 45  20                       |.U.E E E |


## s command eco temperature  setting

	s:AAARAAAACMNJACogPQkHGAM=

	00 00 11 00 00 00 08 c3  49 00 2a 20 3d 09 07 18  |........I.* =...|
	03                                                |.|

## s command ConfigValveFunctions
    
	s:AAQSAAAAD8OAATIM/wA=\r\n
  

	00 04 12 00 00 00 0f c3  80 01 32 0c ff 00
  
It is decoded as following:

	Description        Length      Example Value
	=====================================================================
	Base String        6           000412000000
	RF Address         3           0FC380
	Room nr            1           01
	Boost              1           32
	Decalcification    1           0C
	Valve maximum      1           FF
	Valve offset       1           00

### Boost

    hex:  |    32     |
    dual: | 0011 0010 |
            |||| ||||
            |||+-++++--- valve position: 10010 -> 18 = 18/20 = 90 % (20 is fully open)
            |||
            +++--------- boost duration: 001 -> 1 = duration / 5 minutes
                                       (to get the actual duration, the value must be multiplied by 5: 1 * 5 = 5 minutes)

### Decalcification

    hex:  |    0C     |
    dual: | 0000 1100 |
            |||| ||||
            |||+-++++--- hour: 01100 -> 12
            |||
            +++--------- day: 000: saturday
                              001: sunday
                              010: monday
                              011: tuesday
                              100: wednesday
                              101: thursday
                              110: friday
                              
## s command AddLinkPartner


## s command RemoveLinkPartner


## s command SetGroupAddress


## s command RemoveGroupAddress



# The s Response

	s:00,0,31

This can be decoded as following
	Description        Length      Example Value
	=====================================================================
	Duty Cycle          2           00 
	Command Result      1           0
	Free Memory Slots   2           31

### Duty Cycle

The hex representation of the duty cycle as a percentage.
If cube will queue S commands in memory when % reach 100%.

### Command Result

	0:	command processed
	1:	command discarded

### Free Memory Slots

The hex representation of the free memory slots
