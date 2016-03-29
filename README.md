max-cube-protocol
=================

A attempt to write down the protocol of the eQ3 / ELV MAX! Heating system

This protocol is implemented in various opensource projects.

* Openhab http://openhab.org
	* MAX! Binding https://github.com/openhab/openhab2/tree/master/addons/binding/org.openhab.binding.max
	* maxcul binding https://github.com/openhab/openhab/tree/master/bundles/binding/org.openhab.binding.maxcul
* FHEM http://fhem.de

Various other implementations:
* Javascript decoding https://github.com/ivesdebruycker/maxcube
* Python https://github.com/aleszoulek/maxcube
* MQTT connection https://github.com/leachj/mqtt-maxcube
* CS https://github.com/bietiekay/hacs/tree/master/tools/MAXDebug
* Arduino https://github.com/autopower/thermeq3
* C https://github.com/KnuthLohse/MAXCPP
* NodeJS https://github.com/georg90/max-control


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
