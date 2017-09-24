# ZW Get Node Protocol Info 0x41

A ZW get node protocol info request has a one byte request body consisting of a node ID, and the response includes a six byte response describing properties of the node, as well as its [device class](/devices/classes.md).

**Message Type**: 0x41

**Request Body**:

    -----------
    | Node ID |
    -----------
    |     x?? |
    -----------

**Response Body**:

    ------------------------------------------------------------------
    | Capabilities |  ?A |  ?B | DC Basic | DC Generic | DC Specific |
    ------------------------------------------------------------------
    |          x?? | x?? | x?? |      x?? |        x?? |         x?? |
    ------------------------------------------------------------------

    Capabilities:
    ------------------------------------
    | Bit 7 |                       ?? |
    | Bit 6 |                Listening |
    | Bit 5 |                       ?? |
    | Bit 4 |                       ?? |
    | Bit 3 |                       ?? |
    | Bit 2 |                       ?? |
    | Bit 1 |                       ?? |
    | Bit 0 |                       ?? |
    ------------------------------------

    A device is listening, if it is on and waiting for ZWave requests. AC powered switches will have this bit set to 1. Most battery powered devices, for example remotes, and fire alarms, will have this bit set to 0. These devices often need to be manually turned on to be put into a listening state.

    ?A: ??

    ?B: ??

    DC Basic: the basic device class

    DC Generic: the generic device class

    DC Specific: the specific device class

    **NOTE**: If a node is not found on the network, then the response Capabilities, ?A, ?B are all zero, and the device class is (3, 0, 0).
