
QkProtocol: Specification
====

> Version: 0.1



### Packet Structure

```
HEADER[4] PAYLOAD[]
```

#### Header

```
FLAGS[2] ID[1] CODE[1]
```

```
FLAGS.1 = RSVD[1] SRC[3] RSVD[1] FRAG[1] LASTFRAG[1] RSVD[1]
FLAGS.2 = RSVD[5] DEST[3]
```

| Field    | Value                                          |
|----------|------------------------------------------------|
| SRC      | 0 - QkHost<br>1 - QkDevice<br>2 - QkComm       |
| DEST     | 0 - QkHost<br>1 - QkDevice<br>2 - QkComm       |
| FRAG     | 0 - Not fragmented<br>1 - Fragmented           |
| LASTFRAG | 0 - Not the last fragment<br>1 - Last fragment |


#### Payload
 
 
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


### Byte stuffing

There are two special bytes: SEF (start/ending flag) and DLE (data link escape). The SEF is used to delimit a packet's data (including header and payload) and the DLE byte is used to escape a data byte so it's not erroneously parsed as a *special byte*.  

| Special byte | Hex |
|--------------|:---:|
| SEF          |  55 |
| DLE          |  DD |

To illustrate this, suppose you want to send the following packet:

```
[01] [02] [03]
```
To delimit the packet's data we use the SEF byte, so we get:

```
[55] [01] [02] [03] [55]
```

However, it may easily happen that the flag byte occurs in the data, such as in this packet:

```
[55] [01] [02] [55] [03] [55]
```

In this case, the second ``[05]`` will be erroneously interpreted as the end of the packet, so a DLE is inserted before it:

```
[55] [01] [02] [DD] [55] [03] [55]
```

The escape byte tells the receiver that the byte following it it's not a *special byte*. Hence, there are two bytes that must be escaped: SEF and DLE. This means that when DLE occurs in the packet's data it is also preceeded by the escape byte:

```
[55] [01] [02] [DD] [DD] [03] [55]
```


