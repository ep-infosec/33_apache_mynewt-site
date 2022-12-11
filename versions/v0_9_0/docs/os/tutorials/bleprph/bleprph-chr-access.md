## BLE Peripheral Project

### Characteristic Access

<br>

#### Review

A characteristic's access callback implements its behavior.  Recall that
services and characteristics are registered with NimBLE via attribute tables.
Each characteristic definition in an attribute table contains an *access_cb*
field.  The *access_cb* field is an application callback that gets executed
whenever a peer device attempts to read or write the characteristic.

Earlier in this tutorial, we looked at how *bleprph* implements the GAP
service.  Let's take another look at how *bleprph* specifies the first few
characteristics in this service.

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

As you can see, *bleprph* uses the same *access_cb* function for all the GAP
service characteristics, but the developer could have implemented separate
functions for each characteristic if they preferred.  Here is the *access_cb*
function that the GAP service characteristics use:

<br>

```c
static int
gatt_svr_chr_access_gap(uint16_t conn_handle, uint16_t attr_handle, uint8_t op,
                        union ble_gatt_access_ctxt *ctxt, void *arg)
{
    uint16_t uuid16;

    uuid16 = ble_uuid_128_to_16(ctxt->chr_access.chr->uuid128);
    assert(uuid16 != 0);

    switch (uuid16) {
    case BLE_GAP_CHR_UUID16_DEVICE_NAME:
        assert(op == BLE_GATT_ACCESS_OP_READ_CHR);
        ctxt->chr_access.data = (void *)bleprph_device_name;
        ctxt->chr_access.len = strlen(bleprph_device_name);
        break;

    case BLE_GAP_CHR_UUID16_APPEARANCE:
        assert(op == BLE_GATT_ACCESS_OP_READ_CHR);
        ctxt->chr_access.data = (void *)&bleprph_appearance;
        ctxt->chr_access.len = sizeof bleprph_appearance;
        break;

    case BLE_GAP_CHR_UUID16_PERIPH_PRIV_FLAG:
        assert(op == BLE_GATT_ACCESS_OP_READ_CHR);
        ctxt->chr_access.data = (void *)&bleprph_privacy_flag;
        ctxt->chr_access.len = sizeof bleprph_privacy_flag;
        break;

    case BLE_GAP_CHR_UUID16_RECONNECT_ADDR:
        assert(op == BLE_GATT_ACCESS_OP_WRITE_CHR);
        if (ctxt->chr_access.len != sizeof bleprph_reconnect_addr) {
            return BLE_ATT_ERR_INVALID_ATTR_VALUE_LEN;
        }
        memcpy(bleprph_reconnect_addr, ctxt->chr_access.data,
               sizeof bleprph_reconnect_addr);
        break;

    case BLE_GAP_CHR_UUID16_PERIPH_PREF_CONN_PARAMS:
        assert(op == BLE_GATT_ACCESS_OP_READ_CHR);
        ctxt->chr_access.data = (void *)&bleprph_pref_conn_params;
        ctxt->chr_access.len = sizeof bleprph_pref_conn_params;
        break;

    default:
        assert(0);
        break;
    }

    return 0;
}
```

After you've taken a moment to examine the structure of this function, let's explore some details.

<br>

#### Function signature

<br>

```c
static int
gatt_svr_chr_access_gap(uint16_t conn_handle, uint16_t attr_handle, uint8_t op,
                        union ble_gatt_access_ctxt *ctxt, void *arg)
```

A characteristic access function always takes this same set of parameters and
always returns an int.  The parameters to this function type are documented
below.

<br>

| **Parameter** | **Purpose** | **Notes** |
| ------------- | ----------- | --------- |
| conn_handle   | Indicates which connection the characteristic access was sent over. | Use this value to determine which peer is accessing the characteristic. |
| attr_handle   | The low-level ATT handle of the characteristic value attribute. | Can be used to determine which characteristic is being accessed if you don't want to perform a UUID lookup. |
| op            | Indicates whether this is a read or write operation | Valid values are:<br>*BLE_GATT_ACCESS_OP_READ_CHR*<br>*BLE_GATT_ACCESS_OP_WRITE_CHR* |
| ctxt          | Contains the characteristic value pointer that the application needs to access. | For characteristic accesses, use the *ctxt->chr_access* member; for descriptor accesses, use the *ctxt->dsc_access* member. |

The return value of the access function tells the NimBLE stack how to respond
to the peer performing the operation.  A value of 0 indicates success.  For
failures, the function returns the specific ATT error code that the NimBLE
stack should respond with.  The ATT error codes are defined in
[net/nimble/host/include/host/ble_att.h](https://github.com/apache/incubator-mynewt-core/blob/master/net/nimble/host/include/host/ble_att.h).

<br>

#### Determine characteristic being accessed

<br>

```c
{
    uint16_t uuid16;

    uuid16 = ble_uuid_128_to_16(ctxt->chr_access.chr->uuid128);
    assert(uuid16 != 0);

    switch (uuid16) {
        // [...]
```

This function uses the UUID to determine which characteristic is being
accessed.  There are two alternative methods *bleprph* could have used to
accomplish this task:

* Map characteristics to ATT handles during service registration; use the *attr_handle* parameter as a key into this table during characteristic access.
* Implement a dedicated function for each characteristic; each function inherently knows which characteristic it corresponds to.

All the GAP service characteristics have 16-bit UUIDs, so this function uses
the *ble_uuid_128_to_16()* function to convert the 128-bit UUID to its
corresponding 16-bit UUID.  This conversion function returns the corresponding
16-bit UUID on success, or 0 on failure.  Success is asserted here to ensure
the NimBLE stack is doing its job properly; the stack should only call this
function for accesses to characteristics that it is registered with, and all
GAP service characteristics have valid 16-bit UUIDs.

<br>

#### Read access

<br>

```c
    case BLE_GAP_CHR_UUID16_DEVICE_NAME:
        assert(op == BLE_GATT_ACCESS_OP_READ_CHR);
        ctxt->chr_access.data = (void *)bleprph_device_name;
        ctxt->chr_access.len = strlen(bleprph_device_name);
        break;
```

This code excerpt handles read accesses to the device name characteristic.  The
*assert()* here is another case of making sure the NimBLE stack is doing its
job; this characteristic was registered as read-only, so the stack should have
prevented write accesses.

To fulfill a characteristic read request, the application needs to assign the
*ctxt->chr_access.data* field to point to the attribute data to respond with,
and fill the *ctxt->chr_access.len* field with the length of the attribute data.
*bleprph* stores the device name in read-only memory as follows:

<br>

```c
const char *bleprph_device_name = "nimble-bleprph";
```

The cast to pointer-to-void is a necessary annoyance to remove the *const*
qualifier from the device name variable.  You will need to "cast away const"
whenever you respond to read requests with read-only data.

It is not shown in the above snippet, but this function ultimately returns 0.
By returning 0, *bleprph* indicates that the characteristic data in
*ctxt->chr_access* is valid and that NimBLE should include it in its response
to the peer.

<br>

**A word of warning:** The attribute data that *ctxt->chr_access.data* points to
must remain valid after the access function returns, as the NimBLE stack needs
to use it to form a GATT read response.  In other words, you must not
allocate the characteristic value data on the stack of the access function.
Two characteristic accesses never occur at the same time, so it is OK to use
the same memory for repeated accesses.

<br>

#### Write access

<br>

```c
    case BLE_GAP_CHR_UUID16_RECONNECT_ADDR:
        assert(op == BLE_GATT_ACCESS_OP_WRITE_CHR);
        if (ctxt->chr_access.len != sizeof bleprph_reconnect_addr) {
            return BLE_ATT_ERR_INVALID_ATTR_VALUE_LEN;
        }
        memcpy(bleprph_reconnect_addr, ctxt->chr_access.data,
               sizeof bleprph_reconnect_addr);
        break;
```

This code excerpt handles writes to the reconnect address characteristic.  This
characteristic was registered as write-only, so the *assert()* here is just a
safety precaution to ensure the NimBLE stack is doing its job.

For writes, the roles of the *ctxt->chr_access.data* and *ctxt->chr_access.len*
fields are the reverse of the read case.  The NimBLE stack uses these fields to
indicate the data written by the peer.

Many characteristics have strict length requirements for write operations.
This characteristic has such a restriction; if the written data is not a 48-bit
BR address, the application tells NimBLE to respond with an invalid attribute
value length error.

For writes, the *ctxt->chr_access.data* pointer is only valid for the duration
of the access function.  If the application needs to save the written data, it
should store it elsewhere before the function returns.  In this case, *bleprph*
stores the specified address in a global variable called
*bleprph_reconnect_addr*.
