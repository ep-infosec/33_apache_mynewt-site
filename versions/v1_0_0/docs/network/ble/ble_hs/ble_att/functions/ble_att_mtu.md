## <font color="#F2853F" style="font-size:24pt">ble\_att\_mtu</font>

```c
uint16_t
ble_att_mtu(uint16_t conn_handle)
```

### Description

Retrieves the ATT MTU of the specified connection.  If an MTU exchange for this connection has occurred, the MTU is the lower of the two peers' preferred values.  Otherwise, the MTU is the default value of 23.

### Parameters

| *Parameter* | *Description* |
|-------------|---------------|
| conn\_handle | The handle of the connection to query. |

### Returned values

The specified connection's ATT MTU.
