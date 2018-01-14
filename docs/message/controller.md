# Serial API Get Init Data 0x02

A serial API get init data request has an empty body, and the response includes the serial API version number, bits describing the controller setup, as well as a bitmap of all the node IDs paired to the controller.

**Message Type**: 0x02

**Request Body**: empty

**Response Body**:

    -------------------------------------------------------
    | Version | Capabilities | Number bitfield bytes | ...
    -------------------------------------------------------
    |     x?? |          x?? |                   x1d | ...
    -------------------------------------------------------

    -----------------------------------------------
    ... |               Node id bytes |  ?A |  ?B |
    -----------------------------------------------
    ... | Number bitfield bytes * x?? | x?? | x?? |
    -----------------------------------------------

    Version: of serial API interface

    Capabilities:
    ------------------------------------
    | Bit 7 |                       ?? |
    | Bit 6 |                       ?? |
    | Bit 5 |                       ?? |
    | Bit 4 |                       ?? |
    | Bit 3 | Static Update Controller |
    | Bit 2 |     Secondary Controller |
    | Bit 1 |                       ?? |
    | Bit 0 |                       ?? |
    ------------------------------------

    Node id bytes:
        Byte 0:
        -----------------------------
        | Bit 7 | Node ID 8 Present |
        | Bit 6 | Node ID 7 Present |
        | Bit 5 | Node ID 6 Present |
        | Bit 4 | Node ID 5 Present |
        | Bit 3 | Node ID 4 Present |
        | Bit 2 | Node ID 3 Present |
        | Bit 1 | Node ID 2 Present |
        | Bit 0 | Node ID 1 Present |
        -----------------------------

        Byte 1:
        ------------------------------
        | Bit 7 | Node ID 16 Present |
        | Bit 6 | Node ID 15 Present |
        | Bit 5 | Node ID 14 Present |
        | Bit 4 | Node ID 13 Present |
        | Bit 3 | Node ID 12 Present |
        | Bit 2 | Node ID 11 Present |
        | Bit 1 | Node ID 10 Present |
        | Bit 0 | Node ID  9 Present |
        ------------------------------

        Byte 2 - 28:
        same pattern

    ?A: ??

    ?B: ??

# ZW Get Controller Capabilities 0x05

A ZW get controller capabilities request has an empty body, and the response includes information on the hierarchy of the controller within the ZWave network.

**Message Type**: 0x05

**Request Body**: empty

**Response Body**:

    ----------------
    | Capabilities |
    ----------------
    |          x?? |
    ----------------

    Capabilities:
    ------------------------------------
    | Bit 7 |                       ?? |
    | Bit 6 |                       ?? |
    | Bit 5 |                       ?? |
    | Bit 4 | Static update controller |
    | Bit 3 |              Was primary |
    | Bit 2 |  Static update ID server |
    | Bit 1 |     Non standard home ID |
    | Bit 0 |     Secondary Controller |
    ------------------------------------

# Serial API Get Capabilities 0x07

A serial API get capabilities request has an empty body, and the response includes the version, manufacturer and product information, and a bitmap of supported message types, indexed by message type value. For example if SerialApiGetInitData (message type 0x02) is supported, then bit 1 of byte 0 will be set to 1.

**Message Type**: 0x07

**Request Body**: empty

**Response Body**:

    ---------------------------------------------------------------------
    | Application Version | Application Revision | Manufacturer ID | ...
    ---------------------------------------------------------------------
    |                 x?? |                  x?? |         x?? x?? | ...
    ---------------------------------------------------------------------

    -------------------------------------------------------
     ... Product Type | Product ID | Supported API Bitmap |
    -------------------------------------------------------
              x?? x?? |    x?? x?? |       32 bytes * x?? |
    -------------------------------------------------------

    Application Version:
    Application Revision:
    Manufacturer ID: 2 byte big endian
    Product Type: 2 byte big endian
    Product ID: 2 byte big endian

    Supported API Bitmap:
        Byte 0:
        ------------------------------------------
        | Bit 7 |                             ?? |
        | Bit 6 |    SERIAL_API_GET_CAPABILITIES |
        | Bit 5 |                             ?? |
        | Bit 4 | ZW_GET_CONTROLLER_CAPABILITIES |
        | Bit 3 |                             ?? |
        | Bit 2 |                             ?? |
        | Bit 1 |       SERIAL_API_GET_INIT_DATA |
        | Bit 0 |                             ?? |
        ------------------------------------------

        Byte 1 - 31:
        same pattern

# GetVersion 0x15

A GetVersion request has an empty body, and the response includes information about the version of the controller.

**Message Type**: 0x15

**Request Body**: empty

**Response Body**:

    ---------------------------------
    |           Info | Library Type |
    ---------------------------------
    | String x?? * ? |          x?? |
    ---------------------------------

    Info: null terminated string
    Library Type: 1 byte

# MemoryGetID 0x20

A MemoryGetID request has an empty body, and the response includes information about the home ID and node ID of the controller

**Message Type**: 0x20

**Request Body**: empty

**Response Body**:

    -----------------------------
    |        Home ID  | Node ID |
    -----------------------------
    | x?? x?? x?? x?? |     x?? |
    -----------------------------

    Home ID: 4 bytes big endian
    Node ID: 1 byte
