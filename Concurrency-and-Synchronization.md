Concurrency and Synchronization: Interview Questions and Answers

1. What is concurrency? How is it different from parallelism?

Concurrency is the ability of a system to make progress on multiple tasks during overlapping periods. A processor may switch between tasks, so they take turns and appear to run at the same time.

Parallelism means multiple tasks are actually executing at the same time, typically on different CPU cores.

For example, one cook switching between several dishes is concurrency; several cooks preparing dishes simultaneously is parallelism. Parallelism is one way to achieve concurrency, but concurrency does not require multiple processors or cores.

2. What is a race condition? Give a real-world example.

A race condition occurs when a program's result depends on the timing or order of operations that access shared data, and at least one operation modifies that data. Without coordination, concurrent operations can interfere and produce an incorrect result.

Example: Two people try to withdraw money from the same account at an ATM and a banking app at nearly the same time. If both read the same initial balance before either withdrawal is recorded, both transactions might be approved even though their combined amount exceeds the balance. A lock or atomic transaction can ensure the balance is checked and updated as one indivisible operation.

3. What is the difference between a mutex and a semaphore?

A mutex (mutual-exclusion lock) protects a resource by allowing only one thread or process to hold the lock at a time. It generally has ownership: the thread that locks it is the one expected to unlock it.

A semaphore is a counter used to control access to a resource or coordinate events:

- A binary semaphore has a count of 0 or 1. It can be used for signaling or simple access control, though it does not necessarily have mutex-style ownership.
- A counting semaphore allows up to a specified number of concurrent users, such as a pool of database connections.

Use a mutex to protect a critical section that must have one owner at a time. Use a semaphore to manage a limited number of available resources or signal between tasks.

4. What is starvation? How does it differ from deadlock? Give a real-world example.

Starvation occurs when a task waits indefinitely because other tasks repeatedly get the resource or service it needs. The system continues to make progress, but the starved task does not.

Deadlock occurs when a group of tasks are each waiting for resources held by others in the group, so none of them can proceed.

Example of starvation: At a busy restaurant, a host continually seats newly arriving small parties ahead of one waiting large party. Other customers are being served, but that party may never get a table.

Example of deadlock: Two drivers meet on a narrow, single-lane road where neither can pass. Each waits for the other to reverse, so neither moves.

5. What is priority inversion, and how can we fix it?

Priority inversion happens when a high-priority task is blocked waiting for a resource held by a low-priority task. If medium-priority tasks keep running and prevent the low-priority task from finishing, the high-priority task can be delayed indirectly by those medium-priority tasks.

Common mitigations include:

- Priority inheritance: Temporarily raise the resource-holding task's priority to the priority of the highest-priority task waiting for that resource. Restore its original priority when it releases the resource.
- Priority-ceiling protocols: Assign a resource a priority ceiling and raise a task's priority while it holds that resource, preventing certain forms of inversion and deadlock.
- Keep critical sections short and avoid holding locks while doing slow or blocking work.

6. What is a livelock? Explain.

A livelock occurs when tasks are active and keep responding to one another, but their actions prevent any of them from making useful progress. Unlike deadlock, the tasks are not simply blocked; they continue changing state.

Example: Two people meet in a hallway and both repeatedly step to the same side to let the other pass. They keep moving, but neither gets through. Randomized backoff or a rule that gives one person precedence can resolve the situation.

7. Explain the types of critical sections.

A process is commonly described as having four sections around its work with shared data:

1. Entry section: Requests permission to enter the critical section, for example by acquiring a lock.
2. Critical section: Accesses or updates shared data or a shared resource. It must be protected so concurrent operations do not cause a race condition.
3. Exit section: Releases the lock or otherwise signals that the resource is available.
4. Remainder section: Performs work that does not require access to the shared resource.

A correct critical-section solution should provide mutual exclusion (at most one process is inside at a time), progress (if the section is free, eligible processes can decide who enters without unnecessary delay), and bounded waiting (a process will not be postponed indefinitely).
