max-cube-protocol
=================

An attempt to write down the protocol of the eQ3 / ELV MAX! Heating system

This protocol is implemented in various opensource projects.

* [eq3-max](https://github.com/Juerd/eq3-max) (Perl)
* [FHEM](http://fhem.de/) (Perl)
* [MAX_Boiler_Control](https://github.com/stephenmhall/MAX_Boiler_Control) (Python)
* [max-control](https://github.com/georg90/max-control) (NodeJS)
* [MAXCPP](https://github.com/KnuthLohse/MAXCPP) (C)
* [maxcube](https://github.com/ivesdebruycker/maxcube) (Javascript)
	* [mqtt-maxcube](https://github.com/leachj/mqtt-maxcube)
	* [maxcube-cli](https://github.com/ivesdebruycker/maxcube-cli)
* [maxcube](https://github.com/aleszoulek/maxcube) (Python)
* [Max::Cube](https://github.com/yoyostile/max-cube-ruby) (Ruby)
* [Max::Cube](https://github.com/joconcepts/max-cube) (Ruby)
* [MAX-cube-ctl](https://github.com/pacostiro/MAX-cube-ctl) (C)
* [MAXDebug](https://github.com/bietiekay/hacs/tree/master/tools/MAXDebug) (CS)
* [MaxManager](https://github.com/ababilone/maxmanager) (CS)
* [MAXSharp](https://github.com/bietiekay/MAXSharp/tree/master/MAXSharp) (C#)
* [maxwindownotify](https://github.com/yfauser/maxwindownotify) (Python)
* [node-max](https://github.com/sebbo2002/node-max)
* [node-red-contrib-maxcube](https://github.com/ivesdebruycker/node-red-contrib-maxcube) (Javascript)
* [Openhab](http://openhab.org/)
	* [MAX! Binding](https://github.com/openhab/openhab2/tree/master/addons/binding/org.openhab.binding.max)
	* [maxcul binding](https://github.com/openhab/openhab/tree/master/bundles/binding/org.openhab.binding.maxcul)
* [pymax](https://github.com/ercpe/pymax) (Python)
* [thermeq3](https://github.com/autopower/thermeq3) (Arduino Yún)
* [maxcube-java](https://github.com/spinscale/maxcube-java/) (Java)

General description on how to connect to the cube can be found in [protocol](protocol.md)

Info about discovery can be found  [here](Cube_Discovery.md)

Currently the following messages are described
* [A Message: Factory reset](A-Message.md)
* [C Message: Configuration](C-Message.md)
* [D Message: Decryption](C-Message.md)
* [E Message: Encryption](C-Message.md)
* [F Message: NTP server](F-Message.md) 
* [H Message: Hello](H-Message.md) 
* [L Message: Device List](L-Message.md)
* [M Message: Metadata](M-Message.md)
* [N Message: New device (pairing)](N-Message.md)
* [Q Message: Quit](Q-Message.md)
* [S Message: Send command](S-Message.md)
* [T Command: Delete device](T-Command.md)
* [U Message: URL configuration](U-Message.md)
* [V Message: Date/time configuration](V-Message.md)
* [Z Message: Wake up](Z-Message.md)

If you want to contribute, pull requests are welcome!
