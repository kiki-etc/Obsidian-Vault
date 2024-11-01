In the simplest system, with a single user and one CPU and not connected to a network, the processor is busy only when it is executing either that user’s jobs or the system’s software. However, when there are many users, such as in a multiprogramming environment, or when there are multiple processes competing for attention from a single CPU, the processor must be allocated to each process in a fair and efficient manner.<br>
A ***program*** is an **inactive** unit, composed of multiple ***processes*** (which are active units when being executed).
A ***thread*** is created by a process, and can be executed independently of its parent process. A process can consist of multiple threads. In a threaded environment, a process owns the resources it is allocated and then the process becomes inactive as its threads use said resources.
Multithreading allows applications to manage separate processes with several threads of control.<br>
**Multiprogramming** requires that the processor be allocated to each job or to each process for a period of time and deallocated at an appropriate moment.
# Process Scheduler
After a job has been accepted by the Job Scheduler to run, the Process Scheduler takes over that job; and if the operating systems support threads, the Process Scheduler takes responsibility for this function as well. It is a low-level scheduler that assigns the CPU to execute the individual actions for those jobs placed on the READY queue by the Job Scheduler.<br>
Process Schedulers alternate between CPU cycles and I/O cycles.
## Job, Process and Thread States
As a job, process, or thread moves through the system, its status changes, often from HOLD, to READY, to RUNNING, to WAITING, and then to FINISHED. (WAITING means that the job can't continue until some resource is allocated.)<br>
Threads, however, have more possible states: adding DELAYED and BLOCKED. When an application has the capability of delaying the processing of a thread by a specified amount of time, it causes the thread to transition from RUNNING to DELAYED. When the prescribed time has elapsed, the thread transitions from DELAYED to READY. A thread transitions from RUNNING to BLOCKED when an I/O request is issued. After the I/O request, it returns to a READY state.<br>
Each process in the system is represented by a data structure called a **process control block** (PCB). For threads, it's a TCB.
# Scheduling Policies and Algorithms
Before the OS can schedule tasks, it needs to resolve three limitations: finite resources, some resources are un-shareable once allocated, some resources require operator intervention (they can't be reassigned automatically from job to job).
A good scheduling policy therefore:
- maximizes ***throughput***: runs as many jobs as possible in a given amount of time
- minimizes ***response time***: quickly turns around interactive requests
- minimizes ***turnaround time***: move entire jobs in and out of the system quickly
- minimizes ***waiting time***: move jobs out of the READY queue as quickly as possible
- maximizes CPU efficiency
- ensure fairness for all jobs

A scheduling policy that interrupts the processing of a job and transfers the CPU to another job is called a **preemptive scheduling policy**. The alternative functions without external (to the job) interrupts. In the case of an infinite loop, the process can be stopped whether the policy is preemptive or not.
## First Come First Served
It is non-preemptive. It handles all incoming objets according to their arrival time. It is unacceptable for interactive systems because quick response times are needed. FCFS is implemented using a FIFO queue.<br>
Here, when a new job enters the system, its PCB is linked to the end of the READY queue and it is removed from the front of the READY queue after the job no longer needs the processor. Assuming there's no multiprogramming, turnaround time can be very unpredictable - which can be a problem as the turnaround times vary widely and are seldom minimized.
## Shortest Job Next
It is non-preemptive. It chooses jobs based on the length of their CPU time. In a batch environment, the implementation is simple, CPU time requirement must be known in advance. Does not work well with interactive systems. In an optimal situation, all jobs are available at the same time and CPU estimates time requirements and orders the jobs accordingly.
## Priority Scheduling
It is non-preemptive. Preferential treatment is given to important jobs. No interrupts happen until CPU cycles are completed or a natural wait occurs.
>[!note]- Natural Wait
>A **natural wait** refers to a situation in computing where a process or thread is paused **automatically** because it is waiting for a specific event or resource that is not yet available, without being forced to wait by the system. It occurs when the process cannot proceed until some condition is met, and during this time, the process is essentially idle.

The Process Scheduler manages multiple READY queues. The system administrator and the process manager use different methods of assigning priorities - for example a system admin may use characteristics extrinsic to the job while the processor manager may consider things like memory requirements, time already spent in the system and the number of peripheral devices requested.
## Shortest Remaining Time
Preemptive version of SJN. Here, the processor is allocated to jobs that are closer to completion. It is preemptive because if a new job has a shorter completion time then the current job is interrupted. It is often used in batch systems and does not work in interactive systems. The overhead is worse here as the system monitors CPU time for READY queue jobs and performs multiple context switches.
## Round Robin
This is a preemptive scheduling policy that is used extensively in interactive systems. Here, it uses a time quantum, which is a predetermined time slice. Each job is assigned a time quantum, varying from 100 milliseconds to 1 to 2 seconds. CPU is shared equally among all active processes, which means one job cannot monopolize the system.<br>
It works as follows: a job is placed on the READY queue, the processor selects the first job and assigns it a time quantum and then allocates the CPU. The timer then expires and if the job CPU cycle is greater than the time quantum, the job is preempted and placed at the end of the READY queue. Information is saved in its PCB (assumedly for context switching). If the job CPU cycle is less than the time quantum then the job and all its allocated resources are released back to the user unless there's an interrupt by an I/O request (the job will be halted and placed at the end of the appropriate queue, once the I/O request is complete the job returns to the READY queue.)
A good size for the time quantum is dependent on the system. Some factors to consider are whether it's a time-critical system (response time is key) or an archival system (turnaround time is key).
## Multiple Level Queues
they work in conjunction with several other schemes. It is best when jobs are grouped by common characteristics. The scheduling policy is then based on some predetermined scheme. There are 4 primary methods of moving jobs.
### Case 1: No movement between queues
Rewards high priority jobs, the processor is allocated using FCFS. The processor is only allocated to lower priority jobs only when high priority queues are empty. Good environments here have fewer high priority jobs so more time is spent with low priority jobs.
### Case 2: Movement between queues
The processor adjusts priorities assigned to each job. High priority jobs have a favorable initial priority but they are treated like all other jobs once they're in the system, A quantum interrupt occurs when a job is preempted and then the job is moved to a lower queue. However, priority can be increased if, for example, an I/O request is issued.
A good environment here is one where jobs are handled by cycle characteristics (CPU or I/O) and in interactive systems.
### Case 3: Variable time quantum per queue
Each queue has its own time quantum size where the size of the next queue is twice as long as the size of the previous queue. It has a faster turnaround time for CPU bound jobs, which improves the chances of the jobs finishing faster.
### Case 4: Aging
This ensures that lower-level queue jobs eventually complete execution. The system keeps track of job wait time. If a job is too old, the system moves the job to the next highest queue and this continues until the job reaches the top queue. The advantage here is that it guards against indefinite postponement.
## Earliest Deadline First
This is a preemptive dynamic priority algorithm. It addresses critical processing requirements of real-time systems: deadlines. Jobs priorities can be adjusted while moving through the system. The primary goal is to process all jobs in the order most likely to allow each to run to completion before reaching their respective deadlines.
When jobs have identical deadlines, some other scheme is applied. Priority can change as more important jobs enter the system.
Problems:
- Missed deadlines: total time required for all jobs greater than allocated time until final deadline
- Impossible to predict job throughput: because of changing priority nature
- High overhead due to the continual evaluation of deadlines
# Managing Interrupts
TBA