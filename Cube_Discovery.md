
# UDP communication

Whereas most of the communication with the Cube goes via the TCP, some specific commands require the use of UDP comunication on port 23272

This is for example to discover a Cube on the network, to reset the device, and to set it to firmware update mode. 
 
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
    Request ID         1           >
    Request Type       1           I
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

There are several other UDP request types

    I   Identify
    N   Get network address
    h   get URL information
    c   get network default info
    R   reboot

#### UDP Network request
The network request needs to be made for a specific device hence the request  `"eQ3Max*\0" + serialnumber + "N"`

For a get network address message the response is the following. 

```
    Description        Length      Example Value
    =====================================================================
    Name?              8           eQ3MaxAp
    Serial number      10          KEQ0523864
    Request ID         1           >
    Request Type       1           N
    IP address         4           The 4 bytes of the ip e.g. 192.168.1.9
    Gateway address    4           The 4 bytes of the gateway e.g. 192.168.1.1
    Netmask address    4           The 4 bytes of the netmask e.g. 255.255.255.255
    DNS server 1       4           The 4 bytes of the dns e.g. 8.8.8.8
    DNS server 2       4           The 4 bytes of the dns e.g. 8.8.4.4
```

#### UDP h URL request
The URL request needs to be made for a specific device hence the request  `"eQ3Max*\0" + serialnumber + "h"`

```
00000000  65 51 33 4d 61 78 41 70  4b 45 51 30 35 36 35 30 eQ3MaxAp KEQ05650
00000010  32 36 3e 68 00 50 6d 61  78 2e 65 71 2d 33 2e 64 26>h.Pma x.eq-3.d
00000020  65 2c 2f 63 75 62 65                             e,/cube
```

For a URL request message the response is the following. 

```
    Description        Length      Example Value
    =====================================================================
    Name?              8           eQ3MaxAp
    Serial number      10          KEQ0523864
    Request ID         1           >
    Request Type       1           h
    Port               2           80
    server address     4           max.eq-3.de
    Separator          1           , 
    address path       4           /cube

```
#### UDP c network default config request
The URL request needs to be made for a specific device hence the request  `"eQ3Max*\0" + serialnumber + "c"`

For a c  request message the response is the following. 

```
00000000  65 51 33 4d 61 78 41 70  4b 45 51 30 35 36 35 30 eQ3MaxAp KEQ05650
00000010  32 36 3e 63 c0 a8 00 de  c0 a8 00 01 ff ff 00 00 26>c.... ........
00000020  c0 a8 00 01 00 00 00 00  01 01                   ........ ..

```

```
    Description        Length      Example Value
    =====================================================================
    Name?              8           eQ3MaxAp
    Serial number      10          KEQ0523864
    Request ID         1           >
    Request Type       1           c
    IP address         4           The 4 bytes of the ip e.g. 192.168.0.222
    Gateway address    4           The 4 bytes of the gateway e.g. 192.168.0.1
    Netmask address    4           The 4 bytes of the netmask e.g. 255.255.0.0
    DNS server 1       4           The 4 bytes of the DNS e.g. 192.168.0.1
    Unknown             6  
```
