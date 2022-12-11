## <font color="#F2853F" style="font-size:24pt">GATT Client Definitions</font>

```c
struct ble_gatt_error {
    uint16_t status;
    uint16_t att_handle;
};
```

```c
struct ble_gatt_svc {
    uint16_t start_handle;
    uint16_t end_handle;
    uint8_t uuid128[16];
};
```

```c
struct ble_gatt_attr {
    uint16_t handle;
    uint16_t offset;
    struct os_mbuf *om;
};
```

```c
struct ble_gatt_chr {
    uint16_t def_handle;
    uint16_t val_handle;
    uint8_t properties;
    uint8_t uuid128[16];
};
```

```c
struct ble_gatt_dsc {
    uint16_t handle;
    uint8_t uuid128[16];
};
```

```c
typedef int ble_gatt_mtu_fn(uint16_t conn_handle,
                            const struct ble_gatt_error *error,
                            uint16_t mtu, void *arg);
```

```c
typedef int ble_gatt_disc_svc_fn(uint16_t conn_handle,
                                 const struct ble_gatt_error *error,
                                 const struct ble_gatt_svc *service,
                                 void *arg);
```

```c
/**
 * The host will free the attribute mbuf automatically after the callback is
 * executed.  The application can take ownership of the mbuf and prevent it
 * from being freed by assigning NULL to attr->om.
 */
typedef int ble_gatt_attr_fn(uint16_t conn_handle,
                             const struct ble_gatt_error *error,
                             struct ble_gatt_attr *attr,
                             void *arg);
```

```c
/**
 * The host will free the attribute mbufs automatically after the callback is
 * executed.  The application can take ownership of the mbufs and prevent them
 * from being freed by assigning NULL to each attribute's om field.
 */
typedef int ble_gatt_reliable_attr_fn(uint16_t conn_handle,
                                      const struct ble_gatt_error *error,
                                      struct ble_gatt_attr *attrs,
                                      uint8_t num_attrs, void *arg);
```

```c
typedef int ble_gatt_chr_fn(uint16_t conn_handle,
                            const struct ble_gatt_error *error,
                            const struct ble_gatt_chr *chr, void *arg);
```

```c
typedef int ble_gatt_dsc_fn(uint16_t conn_handle,
                            const struct ble_gatt_error *error,
                            uint16_t chr_def_handle,
                            const struct ble_gatt_dsc *dsc,
                            void *arg);
```
