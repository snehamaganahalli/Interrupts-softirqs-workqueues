**Bottom Halves:**

Bottom halves run immediately after the interrupt returns. The top half is quick and simple and it runs with some or all interrupts disabled.

**Softirq/tasklets:**
softirq's do run in an interrupt context - the "softirq" context; it's just that it's not "hard-irq" context (which is the context when a hardware interrupt occurs).

**tasklet:**
Tasklets provide serialization. Same tasklet will not run concurrently on two different processor.

**Softirq:**
Does not provide serialization. 2 instances of the softirq can run on concurrently on 2 processor.
Thereore, all the shared data needs the appropriate locks.

**Work Queues:**
It will defer the work into a kernel thread. It runs in the 'process context'. Therefore workqueues care schedulable and can sleep.

If the deferred works sleeps then use work queues or else use softirq/tasklet.


**Improtant Note:**

**Interrupt: Interrupt Context
Softirq/tasklet: Softirq context
workqueue: process context.**

**If the process context and the bottomhalf/softirq context share the data, you need to disable the interrupts and obtain the lock before accessing the data. This prevents the dead-lock.**

**Eg:
If you dint disable the interrupts, then when executing the process context and you have acquired a lock, an interrupt may occur, now the softirq will keep of waiting without executing because the process context is holding the lock which is needed by the softirq. And since softirq context has the higher priority than the process context, the softirq context will not be preempted. Therefore, it will result in dead lock.**

```
local_bh_disable() //Disables softirq and tasklet processing on the local processor
local_bh_enable() //Enables softirq and tasklet processing on the local processor
```


