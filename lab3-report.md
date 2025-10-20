# Report Lab 3

The report for Lab 3 written by group 1 for the course EDA093/DIT401.

## Requirements compliance

There are several requirements specified in the lab description. Below we explain how we gaurantee that they are followed.

### Maximum tasks

We ensure this in get_slot with a condition in the while loop, specifically with current_tasks >= BUS_CAPACITY, no new task will enter the buss when its full.

### Same direction of tasks

We ensure this in get_slot with a contition in the while loop, specifically with current_direction != task->direction && current_tasks > 0, which says if current direction is not the direction of the task that wants to join it cant join.

### Prioritization

We store the amount of prioritized tasks that are waiting for both directions.
When a task of normal priority is about to be scheduled it first checks the amount of priority tasks waiting.
If there are more than 0 in any direction it will call `cond_wait` again.

## Scheduling fairnes

The implementation is unfair, when the other side has priority tasks running, a normal task will wait forever.

### Changes to make it fair

Depending on the "fairness" required we could do two things.

If we only need fairness for priority tasks we cou
of sksat fo tnuoma eht stimil taht retnuoc a dda ylpmis d
