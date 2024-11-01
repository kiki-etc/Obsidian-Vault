Here, we follow the evolution of memory management from the simpler forms to four new memory allocation schemes. Each of these removes the restriction of storing the programs contiguously, and most of them eliminate the requirement that the entire program must reside in memory during its execution.
# Paged Memory Allocation
*Paged memory allocation* is based on the concept of dividing jobs into units of equal size. Each unit is called a ==page==. A section of main memory is called a ==page frame=== and sections of a magnetic disk are called ==sectors==. The paged memory allocation scheme is most efficient when the pages, page frames and sectors are all the same size, which is usually determined by the disk's vector size. Because this is the smallest addressable chunk of disk storage, it isn't subdivided even for significantly small jobs.
<br>Before executing a program, a basic Memory Manager prepares it by:
1. determining the number of pages in the program,
2. locating enough empty page frames in main memory,
3. loading all of the program's pages into those frames.

The pages do not have to be in contiguous memory blocks, which means an empty page frame can be used by any page of any job. Additionally, the compaction scheme used for relocatable partitions is eliminated because there is no external fragmentation between page frames.
<br>Each active job has its own **Page Management Table**, which contains the page number and corresponding memory address to the page frame. There's one entry per page and the numbers are sequential.
A simple Memory Map Table has one entry for each page frame and shows its location and its free or busy status.
### Page Displacement
The displacement (also called the offset) is a relative factor that describes the distance between a certain byte and the beginning of it's page frame. Therefore:
```
BYTE_NUMBER_TO_BE_LOCATED / PAGE_SIZE = PAGE_NUMBER
```
This calculation gives only the page number, not the displacement. The displacement can be calculated using the modulus `%` or multiplying the decimal component by the page size.
### Page vs Page Frames
A Page Map Table (PMT) is used to correlate each job's pages with its page frame number so that exact locations of bytes can be found in main memory.
<br>Let's say we are looking for an instruction with a displacement of 76 on Page 2, assuming a page size of 100. TO find the exact location, the following four steps are done (conceptually):
1. Do the arithmetic, $276/100=2 \text{ with a remainder of }76$
	The page number equals the integer part, the displacement equals the remainder
2. Refer to the job's PMT and find out which page frame contains page 2 (in this example, Page Frame 5)
3. Get the address of the beginning of the page frame by multiplying the page frame number by the page frame size (100).
	```
	ADDRESS_PAGE-FRAME = NUMBER_PAGE-FRAME * SIZE_PAGE-FRAME
	ADDRESS_PAGE-FRAME = 5 * 100 = 500
	```
4. Now add the displacement to the starting address of the page frame to compute the precise location of the instruction in the memory.
	```
	INSTR_ADDRESS_IN_MEMORY = ADDRESS_PAGE-FRAME + DISPLACEMENT
	INSTR_ADDRESS_IN_MEMORY = 500 + 76 = 576
	```

A huge advantage of a paging scheme is that it allows jobs to be allocated in noncontiguous memory locations, allowing memory to be used more efficiently. However, there are disadvantages: overhead is increased and internal fragmentation is still a problem, although it only occurs in the last page of each job. The key to the success of this scheme is the size of the page. A page size that is too small will generate very long PMTs, with a corresponding increase in overhead, while a page size that is too large will result in excessive internal fragmentation.
# Demand Paging Memory Allocation
Demand paging introduces the concept of loading only part of the program into memory for processing. Here, the entire job ned not be loaded into memory from the beginning to the end of its processing. When the job begins to run, its pages are brought into memory only as needed and if they're never needed they're never loaded (for example, error handling, menu choices and mutually exclusive modules).
The key to the successful implementation of this scheme is the use of high-speed direct access storage devices.
<br>How and when the pages are passed between main memory and secondary storage depend on predefined policies that determine when to make room for needed pages, and how to do so. The OS relies on tables, such as the Job Table, the PMT, and the Memory Map Table (MMT), to implement the algorithm. With demand paging, there are three new fields for each page in each PMT: one to determine if the page being requested is already in memory, a second to determine if the page contents have been modified while in memory and a third to determine if the page has been referenced most recently.
The memory field tells the system where to find each page. If it's already in memory, the system will be spared the time required  to bring it from secondary storage. The modified field, noting whether or not the page has been changed, is used to save time when pages are removed from main memory and returned to secondary storage. If
 the contents of the page haven’t been modified, then the page doesn’t need to be rewritten to secondary storage. The original, the one that is already there, is correct. The referenced field notes any recent activity, and is used to determine which pages show the most processing activity and which are relatively inactive. This information is used by several page-swapping policy schemes to determine which pages should remain in main memory and which should be swapped out when the system needs to make room for other pages being requested.
 <br>When there is an excessive amount of page swapping between main memory and secondary storage, the operation becomes inefficient. This is called ==thrashing==.
# Page Replacement Policies and Concepts
Two of the most-well known algorithms are first-in first-out (FIFO) and least recently used (LRU). The FIFO policy is based on the assumption that the best page to remove is the one that has been in memory the longest. The LRU policy chooses the least recently accessed to be swapped out.
## First-In First-Out
The FIFO page replacement policy will remove the pages that have been in memory the longest, that is, those that were first in.
Assuming two page frames are being used, when both page frames are occupied, each new page that is brought into memory will cause an **interrupt** and a page swap into secondary storage. A page interrupt, identified by a \*, is generated anytime a page needs to be loaded into memory, whether a page is swapped out or not. Then, we count the number of page interrupts and compute the failure and success rates.
```
Failure-Rate = Number_of_Interrupts / Page_Requests_Made
```
## Least Recently Used
The LRU page replacement policy swaps out the pages that show the least recent activity, figuring that these pages are the least likely to be used again in the immediate future. This is based on the theory of locality.
## Clock Replacement Variation
This is a variation of the LRU policy implemented using a circular queue and uses a pointer to step through the reference bits of the active pages, therefore, simulating a clockwise motion. The algorithm is paced according to the computer's clock cycle. The algorithm checks the reference bit for each page, if it's zero then the page becomes a target for removal. If all the bits are 1, then the pointer must cycle through the circular queue again.
When a page needs to be replaced (because memory is full), the algorithm does the following:
1. The **hand** scans pages in memory one by one, looking at the reference bit.
2. If the reference bit of the current page is 0, it means the page hasn’t been used recently, and the algorithm replaces it with the new page.
3. If the reference bit is 1, the algorithm sets it to 0 (indicating that the page was recently used but will not be given priority next time) and moves the hand to the next page.
4. The process continues until a page with a reference bit of 0 is found, which is then replaced.
5. Repeat

## Bit Shifting Variation
The **bit-shifting variation** is another variation of the LRU page replacement policy. It shifts reference bits to create a history of page usage over time, giving more precise control over which pages to replace.
<br>**Concept:**
In this variation, instead of using a single reference bit, each page has a **byte or word** (multiple bits) associated with it to track its usage history over time. This is more like a **counter or register** where each bit represents whether the page was referenced at different intervals.
<br>**How it Works:**
	**Initial Setup**:
	• Each page has a register, typically 8 bits (a byte), initialized to 0.
	• The rightmost bit represents the most recent reference, and the leftmost bit represents the oldest reference.
	<br>**Time Interval Updates**:
	• At regular intervals (usually after every set number of clock cycles or page accesses), the system shifts all bits in each page’s register **one position to the right**.
	• The most significant bit (leftmost) is discarded, and a new bit (0 or 1) is inserted into the most recent position (rightmost).
	• If the page was referenced during that interval, a **1** is inserted. If it wasn’t, a **0** is inserted.
	<br>**Page Replacement Decision**:
	• When a page needs to be replaced, the algorithm looks at the binary registers of all pages.
	• Pages with fewer 1s (i.e., less recently used or referenced pages) will have lower values in their registers and are more likely to be replaced.
	• Pages with more 1s, especially in the most recent bits, are considered frequently used and will stay in memory.
>[!warning]- The Mechanics of Paging
>Case 3 presents an interesting situation: pages being modified without being referenced. The key lies in how the referenced bit is manipulated by the operating system. When the pages are brought into memory, each is usually referenced at least once. This means that all of the pages will soon have a referenced bit of 1. Of course the LRU algorithm would be defeated if every page indicated that it had been referenced. Therefore, to make sure the referenced bit actually indicates that it was *recently* referenced, the operating system periodically resets it to 0. Then, as the pages are referenced during processing, the bit is changed from 0 to 1, and the LRU policy is able to identify which pages have actually been frequently referenced. As you can imagine, there is one brief instant, just after the bits are reset, in which all of the pages have reference bits of 0 and are vulnerable. But as processing continues, the most -referenced pages soon have their bits reset to 1, so the risk is minimized.

>[!warning]- The Importance of the Working Set
>The working set is a collection of pages residing in memory that can be accessed directly without incurring a page fault. Fortunately, most programs are structured, and this leads to a ***locality of reference*** during the program's execution, meaning that, during any phase of its execution, the program references only a small fraction of its pages.
>We are then faced with questions like "How many pages comprise the working set?" and "What is the maximum number of pages the operating system will allow for a working set?". The latter is particularly important in networked or time-sharing systems which regularly swap jobs for multiple users.
## Segmented Memory Allocation
With this, each job is divided into several segments of different sizes, one for each module that contains pieces that perform related functions (e.g. a subroutine). Because the size of each segment is different, main memory is allocated dynamically. When the program is compiled or assembled, the segments are set up according to the program's structural modules. Each segment is numbered and a Segment Map Table (SMT) is generated for each job.
<br>The Memory Manager needs to keep track of the segments in memory. This is done with three tables, combining aspects of both partitions and demand paging memory management:
- The Job Table lists every job being processed
- The SMT lists details about each segment
- The MMT monitors the allocation of main memory

The segments don't need to be stored contiguously in memory. Additionally, the  displacement cannot be larger than the size of the segment if the program is coded correctly. However, the Memory Manager must always guard against this possibility by checking the displacement against the size of the segment.
## Segmented/Demand Paged Memory Allocation
This allocation scheme doesn't keep each segment as a single contiguous unit, but subdivides it into pages of equal size that are smaller than most segments, and are more easily manipulated than whole segments. Therefore, many of the problems of segmentation - compaction, external fragmentation, and secondary storage handling - are removed because the pages are a fixed length.
It requires 4 tables:
- The Job Table lists every job in process
- The Segment Map Table lists details about each segment
- The PMT lists details about every page
- The MMT monitors the allocation of the page frames in main memory

To access a certain location in memory, a three-dimensional addressing scheme (with the `SEGMENT_NUMBER`, the `PAGE_NUMBER` and the `DISPLACEMENT`) is used. The major disadvantages are twofold: the overhead that is required to manage the tables (Segment Map Tables and the Page Map Tables), and the time required to reference them. To minimize the number of references, many systems take advantage of associative memory to speed up the process.
Associative memory is a name given to several registers that are allocated to each job that is active. Their task is to associate several segment and page numbers, belonging to the job being processed, with their main memory addresses.
# Virtual Memory
This gives the illusion that the computer has much more memory than it actually has. That is, it allows very large jobs to be executed even though they require large amounts of main memory. With virtual memory, even though only a portion of each program is stored in memory at any given moment, by swapping pages into and out of memory, it gives users the appearance that their programs are completely loaded into main memory during their entire processing time.<br>
It can be implemented with both paging and segmentation.
Using paging, it allows internal fragmentation and not external fragmentation; programs are divided into equal sized pages and the absolute address is calculated using the page number and displacement.
With segmentation, it doesn't allow internal fragmentation but allows external fragmentation. Programs are divided into unequal-sized segments that contain logical groupings of code. The absolute address is calculated using the segment number and displacement.<br>
Virtual memory advantages:
- Job's size is no longer restricted to the size of main memory
- Memory is used more efficiently because the only sections of a job stored in memory are those needed immediately
- It allows an unlimited amount of multiprogramming, which can apply to many jobs, as in dynamic and static partitioning; or to many users
- It allows the sharing of code and data
- It facilitates the dynamic linking of program segments

The advantages far outweigh the disadvantages, which include:
- Increased processor hardware costs
- Increased overhead for handling paging interrupts
- Increased software complexity to prevent thrashing
# Cache Memory
Cache memory is based on the concept of using a small, fast, and expensive memory to supplement the workings of main memory. Data or instructions that are frequently accessed are stored here so they can be accessed and processed/executed faster - boosting performance.<br>
A typical microprocessor has two or more cache levels. In a simple configuration, information enters the processor through the bus interface unit, which immediately sends one copy to the Level 2 (L2) cache, which is connected directly to the CPU. A second copy is sent to one of two L1 caches, which are built directly into the chip. On of these L1 caches stores instructions while the other stores data to be used by the instructions.<br>
When designing cache memory, four factors are important:
- cache size
- block size: because of the *principle of locality of reference*, as block size increases, the ratio of the number of references found in the cache to the total number of references will tend to increase
- block replacement algorithm: it is important to replace blocks that are least likely to be used in the future (typically the LRU replacement scheme)
- rewrite policy: when the contents of a block in cache are changed, it must be written back to main memory before it is replaced by another block. rewriting every time there's a change can cause an increase in overhead but waiting for the block to be replaced may be problematic in multiprocessor environments (and in cases where I/O modules can access main memory directly)