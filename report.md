# Group 1 - Lab 2 Report

## Implementation

### Datastructure changes
We modified the thread struct to include two additional fields. 
The fields are sleep_ticks and sleep_start representing how long the thread should sleep and when the thread started sleeping.

### Algorithms
When `timer_sleep` is called we assign the current time to the `sleep_start` and the ticks to the thread struct.
Then we turn of interrupts and call thread_block.

We modified `timer_interrupt` to call a new function, `awake_thread`, for all threads.

`awake_thread` checks first if the thread should be awakened, if so it unblocks it and sets the sleep_start value of the thread to 0 as it's not sleeping anymore.

### Synchronization
The function they call is called `timer_sleep` and a timer is a counter.
The requirements state that a thread is to keep track of how long it should be blocked.
This intuitively means that a thread needs to know when it started sleeping and for how long (or similar).

With a interrupt we can awaken sleeping threads but we need to know when to awaken the sleeping thread. 
We found an already existing interrupt handler and modified it to handle the awakening of the threads.

### Time and space complexity
Time complexity: **O(n)** where n is amount of threads. As described in algorithm part, we will traverse all blocked thread and check its elapse time in the `timer_interupt` function. 

Space complexity: **O(1)**, each thread is storing 2*64 more bits.

##  Test Result
Command `pintos -- -q run alarm-multiple` running result:

```shell
Execution of 'alarm-multiple' complete.
Timer: 893 ticks
Thread: 550 idle ticks, 347 kernel ticks, 0 user ticks
Console: 2952 characters output
Keyboard: 0 keys pressed
Powering off...
=======================================================
Bochs is exiting with the following message:
[ACPI  ] ACPI control: soft power off
=======================================================
```

Before our modifiation, there is no idle tick when we run `pintos -- -q run alarm-multiple` command.

Command `make check` running result: 

```shell
:~/pintos/src/threads/build$ make clean && make check
pass tests/threads/alarm-single
pass tests/threads/alarm-multiple
pass tests/threads/alarm-simultaneous
pass tests/threads/alarm-zero
pass tests/threads/alarm-negative
FAIL tests/threads/batch-scheduler
1 of 6 tests failed.
make: *** [../../tests/Make.tests:27: check] Error 1
```

As the test result shown, we passed 5 of 6 test cases, only batch-scheduler is failed.