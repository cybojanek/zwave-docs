# Device Classes

ZWave devices are categorized by a 3-tuple of (Basic, Generic, Specific).

### Basic

The basic type is the most generic description of a device.

Basic Type        |  Hex | Description
------------------|------|------------
Controller        | 0x01 | Portable controller used to control other nodes, for example a remote.
Static Controller | 0x02 | Static controller in a fixed position, for example a USB Stick Controller.
Slave             | 0x03 | Simple node that cannot initiate transmission of data to other nodes, unless it is a response to a request, for example a light switch. It can only be mains powered.
Routing Slave     | 0x04 | More complicated node that can initate transmission to other nodes, for example a movement detector, or a smarter light switch. Routing slaves can be both mains or battery powered. Mains powered routing slaves can also act as routers between other nodes in the network.

### Generic

The generic type is a more detailed description of a device.

Generic Type        |  Hex | Description
--------------------|------|------------
Generic Controller  | 0x01 | Devices that can control other nodes.
Static Controller   | 0x02 | Similar to a generic controller, but is expected to be stationary.
AV Control Point    | 0x03 | Controls audio-video equipment such as TVs, stereo systems, etc.
Display             | 0x04 | Supports displays showing various data, such as power consumption, temperature, weather forecast, etc.
Network Extender    | 0x05 | ??
Appliance           | 0x06 | ??
Sensor Notification | 0x07 | ??
Switch Thermostat   | 0x08 | Devices that control temperature.
Window Covering     | 0x09 | Devices that cover windows. *Deprecated*
Repeater Slave      | 0x0F | Has no purpose other than to help forward packets between other nodes.
Switch Binary       | 0x10 | Switches that support two states, for example on/off light switches.
Switch MultiLevel   | 0x11 | Switches that support more than two states, for example light dimmers.
Switch Remote       | 0x12 | ??
Switch Toggle       | 0x13 | ??
Z/IP Node           | 0x15 | Z-Wave over IP node
Ventilation         | 0x16 | ??
Security Panel      | 0x17 | ??
Wall Controller     | 0x18 | ??
Sensor Binary       | 0x20 | Supports sensors. *Deprecated*
Sensor MultiLevel   | 0x21 | Supports various sensors.
Meter Pulse         | 0x30 | Supports metering devices that report pulses (to they mean instantaneous usage?), such as water, gas, or electrical meters.
Meter               | 0x31 | Supports metering devices that automatically accumulate usage, such as water, gas, or electrical meters.
Entry Control       | 0x40 | Implements control devices such as door locks.
Semi Interoperable  | 0x50 | ??
Sensor Alarm        | 0xA1 | Supports various alarm sensors, such as smoke, carbon monoxide, carbon dioxide, flooding, heat, etc.
Non Interoperable   | 0xFF | ??

### Specific

The specific type is a more detailed description of a device, and corresponds to a generic type. The byte values of a specific type are not unique, and their meaning depends on the generic type.
