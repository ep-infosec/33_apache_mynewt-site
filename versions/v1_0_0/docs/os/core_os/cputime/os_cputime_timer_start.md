## <font color="F2853F" style="font-size:24pt">os_cputime_timer_start</font>

```c
void os_cputime_timer_start(struct hal_timer *timer, uint32_t cputime)
```
Sets a timer to expire at the specified cputime.  The callback function for the timer is called when the timer expires. 


#### Arguments

| Arguments | Description |
|-----------|-------------|
| `timer` |  Pointer to an initialized hal_timer.
| `cputime` |  The cputime when the timer expires.


#### Returned values
N/A

#### Notes

`timer` must be initialized using the `os_cputime_timer_init()` function before setting up a timer.

#### Example

```c
void
ble_ll_wfr_enable(uint32_t cputime)
{
    os_cputime_timer_start(&g_ble_ll_data.ll_wfr_timer, cputime);
}

```

