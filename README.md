max-cube-protocol
=================

A attempt to write down the protocol of the eQ3 / ELV MAX! Heating system

This protocol is implemented in various opensource projects.

* [FHEM](http://fhem.de/) (Perl)
* [max-control](https://github.com/georg90/max-control) (NodeJS)
* [MAXCPP](https://github.com/KnuthLohse/MAXCPP) (C)
* [maxcube](https://github.com/ivesdebruycker/maxcube) (Javascript)
	* [mqtt-maxcube](https://github.com/leachj/mqtt-maxcube)
* [maxcube](https://github.com/aleszoulek/maxcube) (Python)
* [MAXDebug](https://github.com/bietiekay/hacs/tree/master/tools/MAXDebug) (CS)
* [Openhab](http://openhab.org/)
	* [MAX! Binding](https://github.com/openhab/openhab2/tree/master/addons/binding/org.openhab.binding.max)
	* [maxcul binding](https://github.com/openhab/openhab/tree/master/bundles/binding/org.openhab.binding.maxcul)
* [thermeq3](https://github.com/autopower/thermeq3) (Arduino Yún)

General description on how to connect to the cube can be found in [protocol](protocol.md)

Info about discovery can be found  [here](Cube_Discovery.md)

Currently the following messages are described
* [A Message](A-Message.md)
* [C Message](C-Message.md)
* [F Message](F-Message.md) 
* [H Message](H-Message.md) 
* [L Message](L-Message.md)
* [M Message](M-Message.md)
* [N Message](N-Message.md)
* [Q Message](Q-Message.md)
* [S Message](S-Message.md)
* [T Command](T-Command.md)
* [U Message](U-Message.md)
* [Z Message](Z-Message.md)

If you want to contribute, pull requests are welcome!
