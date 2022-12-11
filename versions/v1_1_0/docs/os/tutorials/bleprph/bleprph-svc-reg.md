## BLE Peripheral Project

### Service Registration

<br>

#### Attribute Set

The NimBLE host uses a table-based design for GATT server configuration.  The
set of supported attributes are expressed as a series of tables that resides in
your C code.  When possible, we recommend using a single monolithic table, as
it results in code that is simpler and less error prone.  Multiple tables
can be used if it is impractical for the entire attribute set to live in one
place in your code.

*bleprph* uses a single attribute table located in the *gatt_svr.c* file,
so let's take a look at that now.  The attribute table is called
`gatt_svr_svcs`; here are the first several lines from this table:

<br>

```c
static const struct ble_gatt_svc_def gatt_svr_svcs[] = {
    {
        /*** Service: GAP. */
        .type               = BLE_GATT_SVC_TYPE_PRIMARY,
        .uuid128            = BLE_UUID16(BLE_GAP_SVC_UUID16),
        .characteristics    = (struct ble_gatt_chr_def[]) { {
            /*** Characteristic: Device Name. */
            .uuid128            = BLE_UUID16(BLE_GAP_CHR_UUID16_DEVICE_NAME),
            .access_cb          = gatt_svr_chr_access_gap,
            .flags              = BLE_GATT_CHR_F_READ,
        }, {
            /*** Characteristic: Appearance. */
            .uuid128            = BLE_UUID16(BLE_GAP_CHR_UUID16_APPEARANCE),
            .access_cb          = gatt_svr_chr_access_gap,
            .flags              = BLE_GATT_CHR_F_READ,
        }, {
    // [...]
```

<br>

As you can see, the table is an array of service definitions (
`struct ble_gatt_svc_def`).  This code excerpt contains a small part of the
*GAP service*.  The GAP service exposes generic information about a device,
such as its name and appearance.  Support for the GAP service is mandatory for
all BLE peripherals.  Let's now consider the contents of this table in more
detail.

A service definition consists of the following fields:

| *Field* | *Meaning* | *Notes* |
| ------- | --------- | ------- |
| type        | Specifies whether this is a primary or secondary service. | Secondary services are not very common.  When in doubt, specify *BLE_GATT_SVC_TYPE_PRIMARY* for new services. |
| uuid128         | The 128-bit UUID of this service. | If the service has a 16-bit UUID, you can convert it to its corresponding 128-bit UUID with the `BLE_UUID16()` macro. |
| characteristics | The array of characteristics that belong to this service.   | |

<br>

A service is little more than a container of characteristics; the
characteristics themselves are where the real action happens.  A characteristic
definition consists of the following fields:

| *Field* | *Meaning* | *Notes* |
| ------- | --------- | ------- |
| uuid128     | The 128-bit UUID of this characteristic. | If the characteristic has a 16-bit UUID, you can convert it to its corresponding 128-bit UUID with the `BLE_UUID16()` macro. |
| access\_cb  | A callback function that gets executed whenever a peer device accesses this characteristic. | *For reads:* this function generates the value that gets sent back to the peer.<br>*For writes:* this function receives the written value as an argument. |
| flags       | Indicates which operations are permitted for this characteristic.  The NimBLE stack responds negatively when a peer attempts an unsupported operation. | The full list of flags can be found under `ble_gatt_chr_flags` in [net/nimble/host/include/host/ble_gatt.h](https://github.com/apache/mynewt-core/blob/master/net/nimble/host/include/host/ble_gatt.h).|

The access callback is what implements the characteristic's behavior.  Access
callbacks are described in detail in the next section:
[BLE Peripheral - Characteristic Access](bleprph-chr-access/).

The service definition array and each characteristic definition array is
terminated with an empty entry, represented with a 0.  The below code listing
shows the last service in the array, including terminating zeros for the
characteristic array and service array.

<br>

```c hl_lines="26 31"
    {
        /*** Alert Notification Service. */
        .type = BLE_GATT_SVC_TYPE_PRIMARY,
        .uuid128 = BLE_UUID16(GATT_SVR_SVC_ALERT_UUID),
        .characteristics = (struct ble_gatt_chr_def[]) { {
            .uuid128 = BLE_UUID16(GATT_SVR_CHR_SUP_NEW_ALERT_CAT_UUID),
            .access_cb = gatt_svr_chr_access_alert,
            .flags = BLE_GATT_CHR_F_READ,
        }, {
            .uuid128 = BLE_UUID16(GATT_SVR_CHR_NEW_ALERT),
            .access_cb = gatt_svr_chr_access_alert,
            .flags = BLE_GATT_CHR_F_NOTIFY,
        }, {
            .uuid128 = BLE_UUID16(GATT_SVR_CHR_SUP_UNR_ALERT_CAT_UUID),
            .access_cb = gatt_svr_chr_access_alert,
            .flags = BLE_GATT_CHR_F_READ,
        }, {
            .uuid128 = BLE_UUID16(GATT_SVR_CHR_UNR_ALERT_STAT_UUID),
            .access_cb = gatt_svr_chr_access_alert,
            .flags = BLE_GATT_CHR_F_NOTIFY,
        }, {
            .uuid128 = BLE_UUID16(GATT_SVR_CHR_ALERT_NOT_CTRL_PT),
            .access_cb = gatt_svr_chr_access_alert,
            .flags = BLE_GATT_CHR_F_WRITE,
        }, {
            0, /* No more characteristics in this service. */
        } },
    },

    {
        0, /* No more services. */
    },
```

<br>

#### Registration function

After you have created your service table, your app needs to register it with the NimBLE stack.  This is done by calling the following function:

```c
int
ble_gatts_register_svcs(const struct ble_gatt_svc_def *svcs,
                        ble_gatt_register_fn *cb, void *cb_arg)
```

The function parameters are documented below.

| *Parameter* | *Meaning* | *Notes* |
| ----------- | --------- | ------- |
| svcs        | The table of services to register. | |
| cb          | A callback that gets executed each time a service, characteristic, or descriptor is registered. | Optional; pass NULL if you don't want to be notified. |
| cb\_arg     | An argument that gets passed to the callback function on each invocation. | Optional; pass NULL if there is no callback or if you don't need a special argument. |

The `ble_gatts_register_svcs()` function returns 0 on success, or a
*BLE_HS_E[...]* error code on failure.

More detailed information about the registration callback function can be found
in the [BLE User Guide](../../../network/ble/ble_intro/) (TBD).

The *bleprph* app registers its services as follows:

```c
    rc = ble_gatts_register_svcs(gatt_svr_svcs, gatt_svr_register_cb, NULL);
    assert(rc == 0);
```

<br>

#### Descriptors and Included Services

Your peripheral can also expose descriptors and included services.  These are
less common, so they are not covered in this tutorial.  For more information,
see the [BLE User Guide](../../../network/ble/ble_intro/).
