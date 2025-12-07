# Summary - Previous Questions and Answers

## Question 1 (10%)
**What is an operating system? What are the four major tasks performed by an operating system? Please explain them briefly.**

**Answer:**
An operating system (OS) is system software that manages computer hardware and provides services for application programs. The four major tasks are:

1. **Process Management**: Creating, scheduling, and terminating processes; managing CPU allocation and context switching.
2. **Memory Management**: Allocating and deallocating memory, implementing virtual memory, and managing memory protection.
3. **File System Management**: Organizing files and directories, managing storage devices, and providing file access operations.
4. **I/O System Management**: Managing input/output devices, handling device drivers, and coordinating data transfer between devices and memory.

---

## Question 2 (10%)
**How could a system be designed to allow a choice of operating system from which to boot? What would the bootstrap program need to do?**

**Answer:**
A boot manager (boot loader) can be implemented to allow OS selection. The bootstrap program needs to:

1. **Load boot manager**: The initial bootstrap loads a boot manager instead of directly loading an OS.
2. **Display menu**: Present a menu of available operating systems to the user.
3. **Load selected OS**: Load the kernel of the selected OS from its partition.
4. **Transfer control**: Transfer control to the selected OS kernel.

The boot manager typically resides in a separate partition or the Master Boot Record (MBR) and can access multiple OS partitions.

---

## Question 3 (10%)
**What is the main advantage of the microkernel approach to system design? How do user programs and system services interact in a microkernel architecture? What are the disadvantages of using the microkernel approach?**

**Answer:**

**Main advantage**: Better security and reliability. The kernel is minimal, and most OS services run as user-level processes, so a failure in one service doesn't crash the entire system.

**Interaction**: User programs and system services communicate through message passing. The microkernel provides only essential services (IPC, memory management, scheduling), while other services (file system, device drivers) run as separate user processes.

**Disadvantages**:
- Performance overhead due to message passing between user space and kernel space
- More complex system design
- Potential performance degradation compared to monolithic kernels

---

## Question 4 (10%)
**Why does an operating system design, in general, use layered approach? Please state its advantages and disadvantages.**

**Answer:**

**Reasons**: Simplifies design, improves maintainability, and provides abstraction. Each layer uses services from lower layers and provides services to upper layers.

**Advantages**:
- Modularity: Each layer can be developed and tested independently
- Easier debugging: Problems can be isolated to specific layers
- Abstraction: Upper layers don't need to know implementation details of lower layers
- Easier maintenance and updates

**Disadvantages**:
- Performance overhead: Multiple layers can add overhead
- Less efficient: May require more function calls and context switches
- Rigid structure: Changes in one layer may affect multiple layers

---

## Question 5 (10%)
**What is a process? What is a thread? Please compare them.**

**Answer:**

**Process**: An executing program with its own memory space, including code, data, stack, and heap. It has a Process Control Block (PCB) and is an independent unit of execution.

**Thread**: A lightweight unit of execution within a process. Threads share the same memory space (code, data, heap) but have their own stack and registers.

**Comparison**:
- **Memory**: Processes have separate memory spaces; threads share memory within a process
- **Creation overhead**: Threads are cheaper to create and switch
- **Communication**: Processes use IPC; threads can directly access shared memory
- **Isolation**: Process failures don't affect others; thread failures can affect the entire process
- **Synchronization**: Threads need synchronization for shared data

---

## Question 6 (10%)
**What resources are used when a thread is created?**

**Answer:**
When a thread is created, the following resources are used:

1. **Stack space**: Each thread needs its own stack for local variables and function calls
2. **Thread Control Block (TCB)**: Stores thread state, registers, and thread-specific information
3. **Registers**: CPU registers for thread execution context
4. **Program counter**: Points to the current instruction
5. **Memory for thread-local storage** (if used)

Note: Threads share the process's code, data, and heap segments, so these don't need to be duplicated.

---

## Question 7 (10%)
**What are the five states of a process in a computer system? Please describe the relationships of these five process states.**

**Answer:**

The five states are:

1. **New**: Process is being created
2. **Ready**: Process is loaded in memory and waiting for CPU
3. **Running**: Process is executing on CPU
4. **Waiting/Blocked**: Process is waiting for an event (I/O, signal, etc.)
5. **Terminated**: Process has finished execution

**Relationships**:
- New → Ready: Process is admitted
- Ready → Running: CPU scheduler selects the process
- Running → Ready: Time slice expires or preempted
- Running → Waiting: Process requests I/O or waits for event
- Waiting → Ready: Event occurs (I/O completes)
- Running → Terminated: Process finishes or is killed

---

## Question 8 (10%)
**Describe the actions taken by a kernel to context-switch between processes.**

**Answer:**

Context switch steps:

1. **Save current process state**: Save CPU registers, program counter, stack pointer, and other context information to the current process's PCB
2. **Update process state**: Change current process state from Running to Ready (or Waiting)
3. **Select next process**: CPU scheduler selects the next process to run
4. **Update process state**: Change selected process state from Ready to Running
5. **Restore process state**: Load saved registers, program counter, stack pointer from the new process's PCB
6. **Update memory management**: Switch page tables and update MMU registers
7. **Resume execution**: Transfer control to the new process

The kernel must ensure atomicity during context switch to prevent data corruption.

---

## Question 9 (10%)
**Why does an operating system have both the user mode and the kernel mode for processes? What are the advantages and disadvantages of the design?**

**Answer:**

**Reasons**: To protect the OS and ensure system stability. Kernel mode has privileged access to hardware and system resources.

**Advantages**:
- **Security**: Prevents user programs from directly accessing hardware or modifying critical system data
- **Stability**: User program errors cannot crash the system
- **Controlled access**: System resources are accessed only through system calls
- **Protection**: Prevents unauthorized access to memory and I/O devices

**Disadvantages**:
- **Performance overhead**: Mode switching (user to kernel) requires time
- **System call overhead**: Each system call involves mode transition
- **Complexity**: Need to carefully design system call interface

---

## Question 10 (10%)
**Please list and describe three major activities of the operating system with regard to memory management, and three major activities of the operating system with regard to secondary storage management.**

**Answer:**

**Memory Management**:
1. **Allocation**: Allocating memory to processes when they need it
2. **Deallocation**: Freeing memory when processes terminate or release memory
3. **Protection**: Ensuring processes can only access their own memory space

**Secondary Storage Management**:
1. **Free space management**: Tracking and managing free blocks on disk
2. **Storage allocation**: Allocating disk space to files
3. **Disk scheduling**: Optimizing the order of disk I/O requests to minimize seek time

---

## Question 11 (10%)
**Can the segmentation and paging mechanisms cooperate with each other? Please give your reasons.**

**Answer:**

**Yes**, they can cooperate. This is called **segmented paging**.

**Reasons**:
- **Segmentation** provides logical division (code, data, stack) and protection
- **Paging** provides physical memory management and eliminates external fragmentation
- **Combination**: Each segment can be divided into pages, combining benefits of both:
  - Logical organization from segmentation
  - Efficient memory allocation from paging
  - Protection at segment level
  - No external fragmentation

**Example**: Intel x86 architecture uses both segmentation and paging, where segments are further divided into pages.

---

## Question 12 (10%)
**What is a Thrashing? How to resolve this problem?**

**Answer:**

**Thrashing**: A condition where a process spends more time swapping pages in and out than executing, leading to low CPU utilization.

**Causes**: Process doesn't have enough frames to hold its working set, causing frequent page faults.

**Solutions**:
1. **Working set model**: Allocate enough frames to hold each process's working set
2. **Reduce degree of multiprogramming**: Suspend or swap out some processes
3. **Local replacement**: Use local page replacement to prevent one process from taking frames from others
4. **Priority-based allocation**: Give more frames to higher priority processes
5. **Page fault frequency**: Monitor and adjust frame allocation based on page fault rate

---

## Question 13 (10%)
**What is Table Lookaside Buffer (TLB)? How it works?**

**Answer:**

**TLB**: A hardware cache that stores recently used page table entries to speed up address translation.

**How it works**:
1. CPU generates virtual address (page number + offset)
2. **TLB lookup**: Check if page number is in TLB
3. **TLB hit**: If found, use frame number from TLB directly (fast)
4. **TLB miss**: If not found, access page table in memory to get frame number, then update TLB
5. Combine frame number with offset to form physical address

**Performance**: TLB hit takes ~1 cycle; TLB miss requires memory access (~100+ cycles). High TLB hit ratio is crucial for performance.

---

## Question 14 (10%)
**What are the major purposes of Table Lookaside Buffer (TLB)? How it works if the memory hierarchy includes L1, L2 and L3 caches?**

**Answer:**

**Major purposes**:
1. **Speed up address translation**: Avoid accessing page table in memory
2. **Reduce memory access**: Reduce the number of memory accesses needed for address translation
3. **Improve performance**: Critical for system performance

**With cache hierarchy**:
1. **TLB lookup first**: Check TLB (fastest, ~1 cycle)
2. **If TLB miss**: Access page table
3. **Cache lookup**: Check L1 cache for page table entry (~1-3 cycles)
4. **If L1 miss**: Check L2 cache (~10 cycles)
5. **If L2 miss**: Check L3 cache (~40 cycles)
6. **If L3 miss**: Access main memory (~100+ cycles)

The TLB operates at the fastest level, and cache hierarchy helps speed up page table access on TLB misses.

---

## Question 15 (10%)
**What is Interrupt? What are its usages?**

**Answer:**

**Interrupt**: A signal from hardware or software that causes the CPU to temporarily halt current execution and transfer control to an interrupt handler.

**Usages**:
1. **I/O completion**: Device signals completion of I/O operation
2. **Timer interrupts**: For time slicing and scheduling
3. **Hardware errors**: Memory errors, power failures
4. **System calls**: Software interrupts to request OS services
5. **Exceptions**: Division by zero, page faults, invalid memory access
6. **Inter-process communication**: Signals between processes

**Benefits**: Allows asynchronous event handling, improves CPU utilization, enables multitasking.

---

## Question 16 (10%)
**How to use interrupt and polling mechanisms to do process manipulation?**

**Answer:**

**Interrupt mechanism**:
- Process initiates I/O and blocks (moves to waiting state)
- CPU continues with other processes
- When I/O completes, device sends interrupt
- Interrupt handler wakes up the waiting process
- Process moves to ready state

**Polling mechanism**:
- Process periodically checks I/O status
- Process remains in running state
- CPU wastes cycles checking status
- Less efficient than interrupts

**Comparison**: Interrupts are more efficient as CPU can do useful work while waiting. Polling wastes CPU cycles but may be used for time-critical operations.

---

## Question 17 (10%)
**What is the concept of copy-on-write? How does it work? What are the primary advantages of using copy-on-write in operating systems?**

**Answer:**

**Concept**: A technique where parent and child processes initially share the same pages in memory. Pages are only copied when one process modifies them.

**How it works**:
1. **Fork()**: Parent and child share all pages, marked as read-only
2. **First write**: When a process tries to write, a page fault occurs
3. **Copy**: OS creates a copy of the page for the writing process
4. **Update**: Mark pages as writable and update page tables

**Advantages**:
1. **Efficient process creation**: Fork() is fast, no immediate copying
2. **Memory savings**: Only modified pages are copied
3. **Better performance**: Reduces memory allocation and copying overhead
4. **Faster startup**: Child processes start quickly

---

## Question 18 (10%)
**Why do we need Direct Memory Access (DMA) mechanism? How does DMA mechanism perform data transfer among memory and I/O devices?**

**Answer:**

**Why needed**: Without DMA, CPU must handle every byte transfer, wasting CPU cycles. DMA allows devices to transfer data directly to/from memory without CPU intervention.

**How it works**:
1. **CPU initiates**: CPU sets up DMA controller with source, destination, and transfer size
2. **DMA takes over**: DMA controller handles data transfer
3. **CPU continues**: CPU can execute other tasks
4. **Interrupt on completion**: DMA sends interrupt when transfer completes
5. **CPU notified**: CPU handles completion

**Benefits**: Reduces CPU overhead, improves system performance, allows concurrent CPU execution and I/O operations.

---

## Question 19 (10%)
**How to use Memory Mapped I/O mechanism to perform file access operations?**

**Answer:**

**Memory-mapped I/O** maps a file directly into a process's virtual address space.

**Process**:
1. **mmap() system call**: Process calls mmap() to map file into memory
2. **OS maps file**: OS creates page table entries pointing to file pages
3. **Access as memory**: Process accesses file data as if it were in memory
4. **Page fault on access**: First access causes page fault, OS loads page from disk
5. **Subsequent accesses**: Direct memory access (may be cached)
6. **Write back**: Modified pages are written back to disk (periodically or on msync())

**Advantages**: Simpler programming model, efficient for large files, OS handles caching and paging automatically.

---

## Question 20 (10%)
**Some file systems are good for small file manipulations while others are good for large file operations. How to design a file system that is good for both small and large file manipulations?**

**Answer:**

**Design strategies**:

1. **Hybrid allocation**: Use different allocation methods for different file sizes
   - Small files: Direct blocks or inode-based storage
   - Large files: Indirect blocks or extent-based allocation

2. **Adaptive block size**: Use smaller blocks for small files, larger blocks for large files

3. **Metadata optimization**: 
   - Efficient inode structure for small files
   - Extent lists for large files

4. **Caching strategies**:
   - Cache metadata for small files
   - Prefetch for large sequential files

4. **Directory structure**: Efficient directory organization for both small and large directories

**Example**: Ext4 uses extents for large files and direct/indirect blocks for smaller files.

---

## Question 21 (10%)
**What are the advantages of the variant of linked allocation that uses a FAT to chain together the blocks of a file?**

**Answer:**

**FAT (File Allocation Table)** advantages:

1. **Random access**: FAT allows direct access to any block without traversing the entire chain
2. **Efficient directory**: Directory entries only need starting block number
3. **Fast file access**: Can quickly find next block from FAT
4. **Easy file extension**: Simple to add new blocks to file
5. **Centralized management**: All block pointers in one table
6. **Cache friendly**: FAT can be cached in memory for fast access
7. **Simple implementation**: Easier than maintaining pointers in each block

**Compared to simple linked allocation**: No need to read each block to find the next one.

---

## Question 22 (10%)
**Explain the role of mmap mechanism. What's the difference between MAP_SHARED and MAP_PRIVATE? When should the mechanism increase and decrease the reference counter of a file?**

**Answer:**

**Role of mmap**: Maps a file or device into process virtual address space, allowing file access as memory operations.

**MAP_SHARED vs MAP_PRIVATE**:
- **MAP_SHARED**: Changes are visible to other processes mapping the same file and are written back to the file
- **MAP_PRIVATE**: Changes are private to the process (copy-on-write), not visible to others, not written back to file

**Reference counter**:
- **Increase**: When a process maps a file (mmap), increment counter
- **Decrease**: When a process unmaps a file (munmap) or terminates, decrement counter
- **Purpose**: Track how many processes have the file mapped; file can be closed when counter reaches zero

---

## Question 23 (10%)
**Discuss situations in which the least frequently used (LFU) page replacement algorithm generates fewer page faults than the least recently used (LRU) page-replacement algorithm. Also discuss under what circumstances the opposite holds.**

**Answer:**

**LFU better than LRU**:
- **Stable access patterns**: When page access frequencies are stable over time
- **Repeated access to same pages**: Pages accessed frequently in the past are likely to be accessed again
- **Long-running processes**: Processes with consistent working sets
- **Example**: Database systems with frequently accessed index pages

**LRU better than LFU**:
- **Changing access patterns**: When recently accessed pages are more likely to be accessed again
- **Locality of reference**: Strong temporal locality where recent access predicts future access
- **Phase changes**: Processes that move between different phases with different page sets
- **Example**: Interactive programs where user actions change which pages are needed

**General**: LRU is usually better for most applications due to temporal locality, but LFU can be better for stable, frequency-based access patterns.

---

## Question 24 (10%)
**Describe the Life Cycle of an I/O request.**

**Answer:**

**Life cycle stages**:

1. **Request initiation**: Application makes I/O system call (read/write)
2. **System call**: OS receives request, validates parameters
3. **Device driver**: OS calls appropriate device driver
4. **I/O scheduling**: Request added to device queue (may be reordered for optimization)
5. **Device operation**: Driver programs device controller, initiates I/O
6. **Data transfer**: 
   - **Programmed I/O**: CPU transfers data byte-by-byte
   - **DMA**: DMA controller transfers data directly
7. **Interrupt**: Device sends interrupt when I/O completes
8. **Interrupt handling**: OS interrupt handler processes completion
9. **Wake process**: Waiting process moved to ready queue
10. **Return to user**: System call returns, data available to application

---

## Question 25 (10%)
**The 4KB page size for virtual memory has been standard since at least the 1960s. Given the increasing size of modern applications and memory hierarchies, is it time to consider larger page sizes, such as 128KB?**

**a) What are the pros and cons of switching to a larger page size? (6%)**

**Answer:**

**Pros**:
- **Larger TLB coverage**: Fewer TLB entries needed, higher TLB hit ratio
- **Fewer page faults**: Larger pages reduce page fault frequency
- **Reduced page table size**: Fewer page table entries
- **Better for large sequential access**: Efficient for applications accessing large contiguous memory

**Cons**:
- **Internal fragmentation**: More wasted space per page (average 50% of page size)
- **Waste for small objects**: Small allocations waste entire large page
- **Slower page fault handling**: Loading larger pages takes more time
- **Reduced flexibility**: Less granular memory management
- **Cache pollution**: Large pages may bring in unnecessary data

**b) What impact would this change have on other parts of the system, such as cache performance, TLB efficiency, and overall memory management? (4%)**

**Answer:**

**TLB efficiency**: 
- **Positive**: Fewer TLB entries needed, higher hit ratio, better performance
- **TLB reach** = TLB size × Page size increases significantly

**Cache performance**:
- **Negative**: May bring in more data than needed, causing cache pollution
- **Positive**: Better prefetching for sequential access patterns

**Memory management**:
- **Positive**: Simpler page table, fewer entries to manage
- **Negative**: More internal fragmentation, less efficient for small allocations
- **Mixed**: Better for large applications, worse for small applications

**Conclusion**: Larger pages benefit large applications but hurt small ones. Modern systems support multiple page sizes (e.g., 4KB, 2MB, 1GB) to optimize for different use cases.

---

## Question 26 (10%)
**In a file system using the inode structure, a large file exceeding 4GB requires triple indirect entries. Consider a database log file, which is typically very large and continuously appends new logs. The most frequently accessed records are usually the most recent ones, located at the end of the file. Since these records are reached through triple indirection, causing multiple slow disk seeks, what strategies can an operating system employ to speed up access to these frequently accessed records?**

**Answer:**

**Strategies**:

1. **Caching indirect blocks**: Cache all indirect blocks (single, double, triple) in memory to avoid disk seeks for index lookups

2. **Pre-allocation**: Pre-allocate blocks for append operations to reduce fragmentation and indirect block updates

3. **Extent-based allocation**: Use extents (contiguous block ranges) instead of individual blocks to reduce indirection levels

4. **Separate log structure**: Use a separate log-structured file system or append-only log structure optimized for sequential writes

5. **Block clustering**: Allocate blocks in clusters near the end of file to reduce seeks

6. **Metadata caching**: Keep frequently accessed indirect blocks in memory cache

7. **Read-ahead**: Prefetch indirect blocks and data blocks for sequential access patterns

8. **Dedicated log file system**: Use specialized file systems (like LFS) designed for append-heavy workloads

9. **Hybrid approach**: Use direct/extent allocation for recent data, indirect for older data

---

## Question 27 (10%)
**Implementing a page table can be challenging, especially with large virtual address spaces (e.g., 48-bit virtual addresses). There are three common approaches for implementing a page table: a) Multiple level indexing; b) Hash table; c) Inverted table. For each approach, please discuss their pros and cons. When discussing, please consider the needed optimizations to make the approach more efficient, and the presence of a decent-sized TLB (Translation Lookaside Buffer).**

**Answer:**

**a) Multiple Level Indexing (Hierarchical Page Tables)**

**Pros**:
- Simple to implement
- Supports sparse address spaces (only allocate needed levels)
- Easy to share pages between processes
- Good with TLB: TLB caches translations, reducing page table walks

**Cons**:
- Multiple memory accesses on TLB miss (one per level)
- Memory overhead for page table structure
- Fixed structure may waste space

**Optimizations**:
- TLB reduces need for page table walks
- Cache page table entries
- Use larger pages to reduce levels needed

**b) Hash Table (Hashed Page Tables)**

**Pros**:
- Efficient for sparse address spaces
- Only stores entries for used pages
- Single hash lookup (with collision handling)
- Good for large address spaces

**Cons**:
- Hash collisions require chaining
- More complex implementation
- May need multiple memory accesses for collision resolution
- Harder to share pages

**Optimizations**:
- TLB handles most translations
- Use good hash function to minimize collisions
- Cache hash table entries
- Clustered page tables for better locality

**c) Inverted Page Table**

**Pros**:
- Minimal memory: One entry per physical page frame
- Size independent of virtual address space
- Efficient for systems with limited physical memory

**Cons**:
- Requires search through table (or hash) for translation
- More complex address translation
- Harder to implement page sharing
- May need multiple memory accesses

**Optimizations**:
- Use hash table to index inverted table
- TLB critical for performance
- Cache frequently used entries
- Hardware support for hash lookup

**Comparison with TLB**: All approaches benefit significantly from TLB, as it caches recent translations and reduces the need for page table walks. A decent-sized TLB (64-1024 entries) can achieve 99%+ hit ratio, making the page table structure less critical for performance.

---

*End of Answers*

