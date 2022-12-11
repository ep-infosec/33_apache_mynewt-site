## <font color="F2853F" style="font-size:24pt">os_cputime_delay_nsecs</font>

```c
void os_cputime_delay_nsecs(uint32_t nsecs)
```
Waits for a specified number of nanoseconds to elapse before returning.


#### Arguments

| Arguments | Description |
|-----------|-------------|
| `nsecs` |  Number of nanoseconds to delay for.


#### Returned values
N/A

#### Notes

#### Example
Delays for 250000 nsecs:
```c
os_cputime_delay_nsecs(250000)
```
