Main memory has gone by several synonyms, including random access memory or RAM, core memory, and primary storage (in contrast with secondary storage, where data is stored in a more permanent way).
# Single User Contiguous Scheme
Here, before execution can begin, each job (in its ***entirety***) is loaded into memory and is given as much ***contiguous*** (i.e. adjacent) space in memory as it needs. If the program is too large to fit into the available memory space, it cannot begin execution.
Only one program at a time is allowed to occupy memory even if there is more space to accommodate the next job.
<br>Single-user systems in a non-networked environment allocate to each user, access to all available memory for each job and jobs are processed sequentially. Allocating memory is done as follows:
1. Evaluate the incoming job. If it is small enough to load into memory, do so. If not, reject it and evaluate the incoming process.
2. Monitor the occupied memory space. Once a job is done executing, indicate that the entire main memory is free.
# Fixed Partitions
Here, the entirety of each "partition" can be assigned to a single job and multiple partitions means multiple jobs can run concurrently. For example, a system with four partitions could hold four jobs in memory at the same time. The partitions are static, so the role of the individual operating the computing system (restarting the entire system to configure partition sizes) was essential.
<br>Once a partition was allocated memory, other jobs had to be prevented from invading its boundaries, which wasn't a problem in Single User Contiguous Schemes.
In a two partition system for example:
1. Check the incoming job's memory requirements. If it's greater than the size of the largest partition, reject the job and go to the next waiting job. If it's less, go to step 2.
2. Check the job size against the size of the first available partition. If it's small enough, check if the partition is free. If it's available, load the job there. If not, step 3.
3. Check the job size against the size of the next available partition. If it's small enough, check if the partition is free. If it is, load the job there. If not, step 4.
4. If neither partition is available, place incoming jobs in the waiting queue for loading at a later time.

This is more flexible than the Single User Contiguous Scheme but it still requires that the entire program be stored contiguously in memory from start to finish of execution.
# Dynamic Partitions
Here, memory was rationed to an incoming job in one contiguous block, with each job given the exact amount of space needed.
This introduced two new fragmentation problems: internal and external fragmentation. When the first jobs are loaded, memory is allocated efficiently. However, as the original jobs are deallocated and new jobs arrive, they are allocated space on a first-come first-served priority basis. The new jobs wouldn't be the same size as the old jobs, therefore the subsequent allocation of memory creates free memory between partitions. This is external fragmentation.
# Best-Fit and First-Fit Allocation
Memory partitions may be allocated on the basis of first-fit memory allocation or best-fit memory allocation. For both schemes, the Memory Manager keeps detailed lists of the free and busy sections of memory either by size or by location. The best-fit allocation method organizes the free/busy lists in order by size, from smallest to largest. The first-fit allocation method organizes the free/busy lists by memory locations, from low-order memory to high-order memory. Each has advantages depending on the needs of the particular allocation scheme. Best-fit usually makes the best use of memory space; first-fit is faster.
# Relocatable Dynamic Partitions
With relocatable dynamic partitions, the Memory Manager relocates programs to gather together all of the empty blocks and compact them to make one block of memory large enough to accommodate some, or all, of the jobs waiting to get in.
<br>The compaction of memory, sometimes referred to as defragmentation, is performed by operating system to reclaim fragmented space.  Most, or all, must be relocated to place them in contiguous memory locations, and then every address, and every reference to an address, within each program must be adjusted to account for the program's new location in memory. The OS must also distinguish between addresses and data values, which is not always easy after a program is loaded onto memory.
## A Machine-Level Look at Relocation
**Memory Relocation and Compaction**  
- **Relocation** involves adjusting the addresses in a program when it is moved in memory. For instance, an instruction like `ADD I, 1` contains an address and data values. If relocated, the address will be adjusted, but the instruction code and data values remain unchanged.
- **Compaction** rearranges memory to combine free spaces, allowing larger jobs to be loaded. The operating system manages this by identifying memory locations and adjusting addresses when moving a program to another location. This is especially important when memory includes loops, decision sequences, or data references.
- The example given shows a program relocated by 200 places. The address is adjusted accordingly, but the instruction code remains the same.

**Figures (Snapshots of Memory)**  
- The diagram illustrates memory before and after compaction:
  1. **(a)** shows external fragmentation before compaction, where free space is scattered across memory (96K total).
  2. **(b)** demonstrates compaction, where the memory is reorganized to eliminate gaps.
  3. **(c)** shows how Job 6 (requiring 84K) is loaded after compaction, filling the memory efficiently.

**Key Points:**
- Relocation ensures the program’s address is updated without altering the data or instruction codes.
- Compaction helps in memory optimization by eliminating fragmentation.
- After compaction, the system updates both the free list and the busy list to reflect the new memory layout and program locations.
### The Essential Role of Registers
Special-purpose registers are used for relocation in systems, two types:
  1. **Bounds Register**: Ensures the program doesn’t access memory locations outside a valid range. If a program tries to access an invalid location, an error occurs (see Figure 2.8 for an example error).
  2. **Relocation Register**: Holds the value that adjusts addresses during relocation. If a program is not relocated, the value is zero.

---
### Example Scenario:
- **Job 4**: Initially loaded into memory at location 30K (starting address = 30720). Job size = 32K (occupies memory 30720 to 63488).
- **LOAD Command Example**: Memory location 31744 has a command that loads the value “ANSWER” into Register 4 for computation.
  - Before relocation, **ANSWER** (value 37) is stored at 53248.
  - **After relocation**:
    - **Job 4** is relocated to memory starting at 18K (18432).
    - Job 4 now occupies memory from 18432 to 51200.
    - The **Relocation Register** stores **-12288**, representing the difference after relocation (30K – 18K = 12K or 12288).

---
### Relocation Adjustments:
Without adjusting memory addresses, instructions like the **LOAD command** at 31744 wouldn’t work after relocation, as the address would be invalid.
<br>**New Adjusted Addresses**:
  - LOAD instruction moves from 31744 to 19456.
  - ANSWER at 53248 relocates to 40960.
  
# <br>Deallocation
When memory is deallocated it creates the opportunity for external fragmentation.
<br>For a fixed partition system, the process is straightforward. When a job within a partition is completed, the Memory Manager immediately deallocates that memory space by resetting the status of the entire memory block from "busy" to "free".
<br>A dynamic partition system uses a more complex algorithm because it tries to combine free areas of memory whenever possible. Therefore the system must be prepared for three alternative solutions:

---
### 1. When the block to be deallocated is adjacent to another free block:
In this case, the system will **merge** the newly deallocated block with the adjacent free block to create a larger free memory area. The purpose of this is to reduce fragmentation by combining contiguous free spaces into a larger single block, which can later accommodate larger memory requests.
<br>**Example:**

| Address | Block Size | Status |
| ------- | ---------- | ------ |
| 1000    | 50 KB      | Free   |
| 1050    | 30 KB      | Used   |
| 1080    | 70 KB      | Free   |

After deallocating the block at address 1050:

| Address | Block Size | Status  |
|---------|------------|---------|
| 1000    | 150 KB     | Free    |

In this case, the 50 KB and 70 KB free blocks merge into a larger 150 KB block.

---
### 2. When the block to be deallocated is between two free blocks:
In this case, when a used block that lies between two free blocks is deallocated, the system can **merge all three blocks**—the two free blocks and the newly freed block—into one larger block. This process helps in minimizing fragmentation and maximizing the available contiguous memory.
<br>**Example:**

| Address | Block Size | Status |
| ------- | ---------- | ------ |
| 1000    | 50 KB      | Free   |
| 1050    | 30 KB      | Used   |
| 1080    | 70 KB      | Free   |

After deallocating the block at address 1050:

| Address | Block Size | Status  |
|---------|------------|---------|
| 1000    | 150 KB     | Free    |

Here, the newly deallocated 30 KB block between two free blocks is merged, resulting in one 150 KB free block.

---
### 3. When the block to be deallocated is isolated from other free blocks:
If the block to be deallocated is not adjacent to any free blocks, the system will simply mark the block as **free**. No merging will take place since there are no adjacent free blocks to combine with. The system will update the memory map to reflect this isolated free block.

**Example:**

| Address | Block Size | Status  |
|---------|------------|---------|
| 1000    | 50 KB      | Used    |
| 1050    | 30 KB      | Free    |
| 1080    | 70 KB      | Used    |

After deallocating the block at address 1000:

| Address | Block Size | Status  |
|---------|------------|---------|
| 1000    | 50 KB      | Free    |
| 1050    | 30 KB      | Free    |
| 1080    | 70 KB      | Used    |

The 50 KB block is marked as free, but no merging occurs.