## The F Message

The F message provides the ntp server information. The f command sets the NTP server. 

The current NTP settings are shown after a sending `f:` to the Cube

    F:ntp.homematic.com,ntp.homematic.com\r\n

by default the `ntp.homematic.com` server seems to be used.

Updates can be done via the `f:[ntp server1],[ntpserver 2]`, Cube responds with an F: message containing the new ntp servers

e.g.

    f:
    F:ntp.homematic.com,ntp.homematic.com
    f:nl.pool.ntp.org,ntp.homematic.com
    F:nl.pool.ntp.org,ntp.homematic.com


