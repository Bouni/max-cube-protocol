# eQ3/ELV MAX! Cube Protocol

## Disclaimer

The main info about the eQ3 Cube Protocol comes from these sources:

- http://www.domoticaforum.eu/viewtopic.php?f=66&t=6654
- https://github.com/bietiekay/hacs/tree/master/tools/MAXDebug
- decompiling the Original MAX Software from eQ3/ELV
- http://www.adreamerslair.nl/category/homeautomation/

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

