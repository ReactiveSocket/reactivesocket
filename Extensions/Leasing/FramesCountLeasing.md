# Frames Counting Leasing Strategy

_This extension specification is currently incubating.  While incubating the version is 0._

## Introduction
Lease provides a powerful mechanism for `Server` to control its stability. However, one common strategy for Leasing may not be efficient for all cases. A common mechanism for authenticating is using a bearer token. Frame Counting is a mechanism in order to achieve stable memory consumption by controlling the number of incoming. This Lease Strategy provides a standardized negotiating mechanism allowed Frame Count using [Leasing Frame][lf].


### Frame Contents

```
     0                   1                   2                   3
     0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |                         Stream ID = 0                         |
    +-----------+-+-+---------------+-------------------------------+
    |Frame Type |0|M|     Flags     |
    +-----------+-+-+---------------+-------------------------------+
    |0|                       Time-To-Live                          |
    +---------------------------------------------------------------+
    |0|                     Number of Frames                        |
    +---------------------------------------------------------------+
                                Metadata
```

* __Frame Type__: (6 bits) 0x02 
* __Flags__: (10 bits)
     * (__M__)etadata: Metadata present
* __Time-To-Live (TTL)__: (31 bits = max value 2^31-1 = 2,147,483,647) Unsigned 31-bit integer of Time (in milliseconds) for validity of LEASE from time of reception. Value MUST be > 0. 
* __Number of Frames__: (31 bits = max value 2^31-1 = 2,147,483,647) Unsigned 31-bit integer of Number of Frames that may be sent until next LEASE. Value MUST be >= 0. 

A Responder implementation MAY stop all further frames by sending a LEASE with a value of 0 for __Number of Frames__.

When a LEASE expires due to time, the value of the __Number of Frames__ that a Requester may make is implicitly 0.

This frame only supports Metadata, so the Metadata Length header MUST NOT be included, even if the (M)etadata flag is set to true.

[wk]: WellKnownLeaseStrategies.md