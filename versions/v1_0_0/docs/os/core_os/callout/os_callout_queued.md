## <font color="#F2853F" style="font-size:24pt">os_callout_queued</font>

```c
    int os_callout_queued(struct os_callout *c)
```

Tells whether the callout has been armed or not.


#### Arguments

| Arguments | Description |
|-----------|-------------|
| `c` |  Pointer to callout being checked  |

#### Returned values

0: timer is not armed

non-zero: timer is armed



