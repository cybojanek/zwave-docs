# General overview

[Z-Wave](https://en.wikipedia.org/wiki/Z-Wave) is a wireless communications protocol used primarily for home automation.

A Z-Wave network consists of Z-Wave devices, also known as *nodes*. A node is *included* into a network through a pairing process with the network *controller*, and can only belong to one network at a time. Each node is identified by a unique one byte node ID assigned during the pairing process. Z-Wave nodes form a *mesh* network, and route messages to each other, which means that the controller does not have to be in range of all the nodes.

A typical controller is a Z-Wave USB Stick that can be plugged into any computer, and shows up as a USB Serial device. Applications interact with nodes through the controller, by sending and receiving Z-Wave [messages](message/format) encapsulated in Z-Wave [packets](packet/format).

Every Z-Wave device belongs to a [device class](device/classes) and implements a number of [command classes](command/classes). For example, a light switch might belong to the *SwitchBinary* device class, and implement the *SwitchBinary* and *Meter* command classes. This means that it can be turned on/off, and report elctrical power consumption.

Z-Wave devices can be either mains or battery powered. Mains powered devices, for example most light switches, are called *listening*, because they can receive commands at any time, and act as routers, extending the mesh network. On the other hand, battery powered devices, for example smoke alarms, remotes, or motion sensors, are *sleepy* and remain in a low power state. They do not act as routers, and must be woken up in order to listen to commands. They will however broadcast messages when they are triggered, for example, when a smoke alarm detects smoke.

# Messaging

Interacting with Z-Wave nodes is complicated by the fact that there are both synchronous and asynchronous messages. Synchronous messages immediately return the full result, for example when requesting the list of nodes on a network ([Serial API Get Init Data](message/controller/#serial-api-get-init-data-0x02)). Asynchronous messages immediately return only a brief status result, indicating if the message was successfully sent out, and the full result is received at a later time. For example, toggling a light switch involves sending a *Basic Command Set*. Once the light switch processes the request, it will respond with a *Basic Command Report*, which indicates its new status. Nodes can also send messages on their own, for example when a sensor is tripped. This means that at the applicaton level, the Z-Wave library implementation must have some kind of routing and callback support.