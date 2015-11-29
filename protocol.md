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
    OUTGOING RESET:                       "a:"
    OUTGOING RESET ERROR:                 "r:"
    OUTGOING DELETE DEVICES:              "t:"
    OUTGOING SET PUSHBUTTON CONFIG:       "w:"
    OUTGOING GET DEVICE LIST:             "l:"
    OUTGOING GET CONFIGURATION:           "c:"
    OUTGOING TIME CONFIG:                 "v:"
    OUTGOING NTP SERVER:                  "f:"
    OUTGOING SEND WAKEUP:                 "z:"

