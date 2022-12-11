## <font color="F2853F" style="font-size:24pt">NimBLE Host GAP Reference</font>

### Introduction

The Generic Access Profile (GAP) is responsible for all connecting, advertising, scanning, and connection updating operations.

### Header

```c
#include "host/ble_hs.h"
```

### Definitions

[BLE host GAP definitions](definitions/ble_gap_defs.md)

### Functions

| Function | Description |
|----------|-------------|
| [ble_gap_adv_active](functions/ble_gap_adv_active.md) | Indicates whether an advertisement procedure is currently in progress. |
| [ble_gap_adv_rsp_set_data](functions/ble_gap_adv_rsp_set_data.md) | Configures the data to include in subsequent scan responses. |
| [ble_gap_adv_rsp_set_fields](functions/ble_gap_adv_rsp_set_fields.md) | Configures the fields to include in subsequent scan responses. |
| [ble_gap_adv_set_data](functions/ble_gap_adv_set_data.md) | Configures the data to include in subsequent advertisements. |
| [ble_gap_adv_set_fields](functions/ble_gap_adv_set_fields.md) | Configures the fields to include in subsequent advertisements. |
| [ble_gap_adv_set_phys](functions/ble_gap_adv_set_phys.md) | <font color="red"> [experimental]</font> Configures primary and secondary PHYs to use in subsequent extended advertisements from Bluetooth 5. |
| [ble_gap_adv_set_tx_power](functions/ble_gap_adv_set_tx_power.md) | <font color="red"> [experimental]</font> Configures Tx Power level to use in subsequent extended advertisements from Bluetooth 5. |
| [ble_gap_adv_start](functions/ble_gap_adv_start.md) | Initiates advertising. |
| [ble_gap_adv_stop](functions/ble_gap_adv_stop.md) | Stops the currently-active advertising procedure. |
| [ble_gap_conn_active](functions/ble_gap_conn_active.md) | Indicates whether a connect procedure is currently in progress. |
| [ble_gap_conn_cancel](functions/ble_gap_conn_cancel.md) | Aborts a connect procedure in progress. |
| [ble_gap_conn_find](functions/ble_gap_conn_find.md) | Searches for a connection with the specified handle. |
| [ble_gap_conn_rssi](functions/ble_gap_conn_rssi.md) | Retrieves the most-recently measured RSSI for the specified connection. |
| [ble_gap_connect](functions/ble_gap_connect.md) | Initiates a connect procedure. |
| [ble_gap_ext_connect](functions/ble_gap_ext_connect.md) |  <font color="red"> [experimental]</font> Same as above but using extended connect from Bluetooth 5. |
| [ble_gap_disc](functions/ble_gap_disc.md) | Performs the Limited or General Discovery Procedures. |
| [ble_gap_ext_disc](functions/ble_gap_ext_disc.md) |  <font color="red"> [experimental]</font>  Same as above but using extended advertising from Bluetooth 5. |
| [ble_gap_disc_active](functions/ble_gap_disc_active.md) | Indicates whether a discovery procedure is currently in progress. |
| [ble_gap_disc_cancel](functions/ble_gap_disc_cancel.md) | Cancels the discovery procedure currently in progress. |
| [ble_gap_security_initiate](functions/ble_gap_security_initiate.md) | Initiates the GAP encryption procedure. |
| [ble_gap_set_event_cb](functions/ble_gap_set_event_cb.md) | Configures a connection to use the specified GAP event callback. |
| [ble_gap_terminate](functions/ble_gap_terminate.md) | Terminates an established connection. |
| [ble_gap_update_params](functions/ble_gap_update_params.md) | Initiates a connection parameter update procedure. |
| [ble_gap_wl_set](functions/ble_gap_wl_set.md) | Overwrites the controller's white list with the specified contents. |
| [ble_gap_set_priv_mode](functions/ble_gap_set_priv_mode.md) | Set privacy mode for peer device. |
| [ble_gap_read_le_phy](functions/ble_gap_read_le_phy.md) | Read PHY on the connections. |
| [ble_gap_set_prefered_default_le_phy](functions/ble_gap_set_prefered_default_le_phy.md) | Set default prefered PHY mode for new connections. |
| [ble_gap_set_prefered_le_phy](functions/ble_gap_set_prefered_le_phy.md) | Set prefered PHY mode for the connections. |
