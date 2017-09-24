# Packet Format

ZWave libraries communicate with devices using single and multi byte packets sent to and from ZWave USB controllers. When plugged into a computer, controllers show up as serial devices i.e. as */dev/tty.usbmodem1451* on OSX or */dev/ttyUSB0* on Linux.

### Single Byte

Single byte packets consist of only a preamble:

* **ACK** 0x06 (acknowledged)
* **NAK** 0x15 (not acknowledged)
* **CAN** 0x18 (can't acknowledge)

```
------------
| Preamble |
------------
|     0x?? |
------------
```

### Multi Byte

Multi byte packets consist of:

* **Preamble**: SOF 0x01 (start of frame)
* **Length**: which is the one byte length of the rest of the message (packet type, message type, body, checksum), and does not include the preamble nor the length byte itself. Since a packet type, message type, and checksum need to always be included, the length can be simplified to *3 + len(body)*.
* **Packet Type**: one of REQUEST 0x00 or RESPONSE 0x01
* **Message Type**: describes the functional message itself, ie request list of nodes, send data to a node etc...
* **Body**: The payload of the message. Can be between 0 and 252 bytes long (inclusive).
* **Checksum**: an XOR starting at 0xff, over everything after the preamble (not including the checksum itself).

```
--------------------------------------------------------------------
| Preamble | Length | Packet Type | Message Type | Body | Checksum |
--------------------------------------------------------------------
|     0x01 |   0x?? |        0x?? |         0x?? |  ??  |    0x??  |
--------------------------------------------------------------------
```

# Packet Sequence

ZWave requests and responses need to be acknowledged, and will be retransmitted if ACKs are not received. Every request results in a coresponding response. The response message type always matches the request message type.

### Standard Request and Response

A standard ZWave request and response proceeds as follows:

    ZWave Library                            USB Controller
                        SOF (Request)
                       -------------->

                             ACK
                       <--------------

                        SOF (Response)
                       <---------------

                             ACK
                       -------------->

### Missing ACK

If the ZWave library does not respond with an ACK in a timely manner, the USB Controller will retransmit the response:

    ZWave Library                            USB Controller
                        SOF (Request)
                       -------------->

                             ACK
                       <--------------

                        SOF (Response)
                       <---------------

                             ...

                        SOF (Response)
                       <---------------

                             ACK
                       -------------->

### CAN Retransmit

If the controller is processing a request or resposne and another request is sent by the ZWave library, the controller will respond with a CAN, and the ZWave library will have to retransmit the request. This can occur in a few scenarios. For example, the client sends two SOF requests, before waiting for an SOF response:

    ZWave Library                            USB Controller
                        SOF (Request A)
                       ---------------->

                              ACK
                       <----------------

                        SOF (Request B)
                       ---------------->

                              CAN
                       <-----------------

                        SOF (Response A)
                       <-----------------

                              ACK
                       ----------------->

                        SOF (Request B)
                       ------------------>

                              ACK
                       <----------------

                        SOF (Response B)
                       <-----------------

                              ACK
                       ----------------->

Another CAN sceniario will occur, if the ZWave library sends a request while the USB Controller is processing an un-ACKed response from a device not triggered by the ZWave library. For example, when a light switch is pressed, a message is sent to the controller. In this case, the ZWave library will likewise receive a CAN and need to retransmit the request. This situation can be detetected by checking for a mismatch between the request and response message types or by receiving an SOF response before an ACK.

    ZWave Library                            USB Controller
                        SOF (Request A)
                       ---------------->

                        SOF (Response X)
                       <-----------------

                              ACK
                       ----------------->

                              CAN
                       <-----------------

                        SOF (Request A)
                       ------------------>

                              ACK
                       <----------------

                        SOF (Response A)
                       <-----------------

                              ACK
                       ----------------->
