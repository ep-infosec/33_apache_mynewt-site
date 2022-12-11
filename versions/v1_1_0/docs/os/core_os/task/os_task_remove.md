## <font color="F2853F" style="font-size:24pt"> os_task_remove</font>

```c
int os_task_remove(struct os_task *t)
```
Removes a task, `t`, from the task list. A task cannot be removed when it is in one of the following states:

* It is running - a task cannot remove itself.
* It has not been initialized.
* It is holding a lock on a semphore or mutex.
* It is suspended waiting on a lock (semaphore or mutex) or for an event on an event queue.

<br>
#### Arguments

| Arguments | Description | 
|-----------|-------------| 
| `t` | Pointer to the `os_task` structure for the task to be removed |

<br>
#### Returned values
`OS_OK`:  Task `t` is removed sucessfully.
<br>`OS_INVALID_PARM`:  Task `t` is the calling task. A task cannot remove itself.
<br>`OS_NOT_STARTED`:  Task `t` is not initialized.
<br>`OS_EBUSY`: Task `t` is either holding a lock or suspended waiting for a lock or an event.
<br>
#### Example

```c

struct os_task worker_task;

int
remove_my_worker_task(void)
{
    /* Add error checking code to ensure task can removed. */
    
    /* Call os_task_remove to remove the worker task */
    rc = os_task_remove(&worker_task);
    return rc;
}
```

