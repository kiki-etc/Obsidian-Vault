Process Synchronization is important because without it, the following types of problems can occur:<br>
**Deadlock**
A situation in which two or more processes are stuck, each waiting for the other to release a resource, and none of them can proceed. **In computing**, this happens when processes are waiting for resources that are held by each other, causing a permanent halt in execution. An example is when a non-shareable or non-preemptable resource is required by two jobs.<br>
**Livelock**
A situation where processes keep changing their states in response to each other but still make no progress. Unlike deadlock, they are not stuck, but they keep reacting in ways that prevent forward movement. **In computing**, livelock happens when processes are active, continuously adjusting their actions, but they never reach their goals because they keep reacting to the changes made by each other.<br>
**Starvation**
A situation in which a process is waiting indefinitely because it never gets the resources it needs, as other higher-priority processes keep taking them. **In computing**, starvation occurs when a process is always bypassed by other processes with higher priority or greater access to resources, preventing it from ever getting what it needs to proceed.
# Deadlocks on File Requests