
QkProtocol: Specification
====

> Version: 0.1



### Packet Structure

```
SEF HEADER[] PAYLOAD[] SEF
```

#### Byte stuffing
There are two special bytes: SEF (start/ending flag) and DLE (data link escape). The SEF is used to delimit a packet's data (including header and payload) and the DLE byte is used to escape a data byte so it's not erroneously parsed as a *special byte*.  
| Special byte | Hex |
|--------------|:---:|
| SEF          |  55 |
| DLE          |  DD |
To illustrate this, suppose you want to send the following data:

```
[01] [02] [03]
```
To delimit the packet we use the SEF byte, so we get:
```
[55] [01] [02] [03] [55]
```

SEF
: Start/ending flag byte, delimits a packet

FLAGS
: Control flags

CODE
 : Meaning of the packet
| Code        | Hex | Payload |
|-------------|:---:|---------|
| OK          |  01 |         |
| ERR         |  FF |         |
| ACK         |  03 |         |
| READY       |  0D |         |
| SEARCH      |  06 |         |
| START       |  0A |         |
| STOP        |  0F |         |
| INFO_QK     |  B1 |         |
| INFO_SAMP   |  B2 |         |
| INFO_BOARD  |  B5 |         |
| INFO_COMM   |  B6 |         |
| INFO_DEVICE |  B7 |         |
| INFO_DATA   |  BD |         |
| INFO_EVENT  |  BE |         |
| INFO_CONFIG |  BC |         |
| DATA        |  D0 |         |
| EVENT       |  DE |         |
| ACTION      |  DA |         |
| STRING      |  DF |         |
| SET_NAME    |  34 |         |
| SET_SAMP    |  36 |         |
| SET_CONFIG  |  3C |         |
