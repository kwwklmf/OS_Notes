# Operating Systems Exam – Questions & Answers
*(All questions from the provided summary)*  
:contentReference[oaicite:0]{index=0}

---

## 1. What is an operating system & four major tasks?
**OS:** Manages hardware, provides environment for application execution.  
**Tasks:**  
1) **Process management** – create, schedule, terminate processes.  
2) **Memory management** – allocate, protect, swap memory.  
3) **File-system management** – organize files, directories, I/O.  
4) **I/O & device management** – buffer, schedule, control devices.

---

## 2. Design a system that allows OS selection at boot; bootstrap responsibilities
**Design:** Use a bootloader (e.g., GRUB) supporting multi-OS menus.  
**Bootstrap must:** Locate OS kernels, load them into memory, set CPU state, and transfer control to the chosen kernel.

---

## 3. Advantage of microkernel; interaction; disadvantages
**Advantage:** Better modularity, reliability, small trusted computing base.  
**Interaction:** User programs interact via message passing to servers.  
**Disadvantages:** Higher overhead due to IPC; slower performance.

---

## 4. Why layered OS design? Pros & cons
**Why:** To structure OS into well-defined layers that depend only on lower ones.  
**Pros:** Easier debugging, maintenance, modularity.  
**Cons:** Reduced efficiency; rigid structure may force unnecessary abstraction steps.

---

## 5. Process vs thread
**Process:** Executing program with its own address space & resources.  
**Thread:** Lightweight execution unit within a process sharing memory.  
**Comparison:** Threads share resources → faster creation, but less isolated.

---

## 6. Resources used when creating a thread
Thread control block, stack, program counter, registers, scheduling info; shares code & address space with process.

---

## 7. Five process states & relationships
States: **New → Ready → Running → (Waiting → Ready) → Terminated**.  
Processes move between Ready/Running based on scheduling; Waiting when blocked.

---

## 8. Kernel actions during context switch
Save current CPU registers & PC; update PCB; load next process’s PCB; restore registers; switch memory context.

---

## 9. Why user mode & kernel mode? Pros & cons
**Why:** Protect hardware and kernel from user-level errors.  
**Pros:** Safety, stability.  
**Cons:** Mode switching overhead; limits direct hardware access.

---

## 10. Three OS activities for memory management & secondary storage
**Memory:** Allocation, protection, swapping/virtual memory management.  
**Secondary storage:** Free-space management, allocation policies, disk scheduling.

---

## 11. Can segmentation and paging cooperate?
Yes. Segmentation defines logical units; each segment can be paged for efficient physical memory use.

---

## 12. What is thrashing? How to resolve?
**Thrashing:** Excessive paging causing performance collapse.  
**Fixes:** Increase RAM, use working-set model, limit degree of multiprogramming.

---

## 13. What is TLB? How it works?
**TLB:** Cache for page-table entries.  
**Works:** On memory access, check TLB → hit returns frame; miss triggers page-table lookup and TLB update.

---

## 14. TLB purposes & behavior with L1/L2/L3 caches
**Purposes:** Speed up address translation; reduce page-table lookups.  
**With caches:** TLB translates VA→PA; caches then service physical-memory access. Miss in TLB still requires page-table walk before cache access.

---

## 15. What is an interrupt? Usages?
Hardware/software signal causing CPU to stop current task and run ISR.  
Used for I/O completion, errors, timers, system calls.

---

## 16. How interrupt & polling handle process manipulation?
**Interrupt:** Device signals completion → OS wakes waiting process.  
**Polling:** CPU repeatedly checks device status → inefficient but simple.

---

## 17. Copy-on-write concept, how it works, advantages
**Concept:** Parent & child share memory until one writes.  
**Works:** Mark pages read-only; upon write fault → create private copy.  
**Advantages:** Saves memory, speeds fork operations.

---

## 18. Why DMA? How it transfers data?
**Why:** Offload CPU from large data movements.  
**How:** DMA controller copies data between device/memory directly, interrupting CPU on completion.

---

## 19. Using Memory-Mapped I/O for file access
Map file blocks into virtual memory → read/write becomes normal memory access; OS handles paging to disk.

---

## 20. Designing FS good for small & large files
Use hybrid structure:  
- Small files stored in inode or small direct blocks.  
- Large files use extents or multilevel indexing.  
This balances low overhead & scalability.

---

## 21. Advantages of linked allocation variant using FAT
FAT keeps block chain in memory → fast sequential and random access; avoids pointer storage in disks; simpler recovery.

---

## 22. Role of mmap, MAP_SHARED vs MAP_PRIVATE & refcount rules
**mmap:** Maps file into memory for direct access.  
**MAP_SHARED:** Writes go to file; increase file refcount on mapping.  
**MAP_PRIVATE:** Copy-on-write; does not propagate writes; refcount increases on mapping, decreases on munmap/close.

---

## 23. LFU vs LRU page-fault behaviors
**LFU better:** When pages used steadily and frequently over long periods.  
**LRU better:** When locality changes; LFU gets stuck with old “frequent” pages.

---

## 24. Life cycle of an I/O request
Request issued → queued → scheduled → device performs I/O → interrupt → OS completes request → wake waiting process.

---

## 25. Larger page size (e.g., 128KB): pros & cons
**Pros:** Smaller page table, fewer TLB misses, faster sequential access.  
**Cons:** More internal fragmentation; wasted memory; inefficient for small/random accesses.

### 25(b). Impact on system
Larger page →  
- **Cache:** May increase pollution due to larger working set chunks.  
- **TLB:** Higher coverage → better performance.  
- **Memory management:** Larger swap units; fewer pages but bigger faults.

---

## 26. Speeding access to large triple-indirect DB log file
Use:  
- Extents or contiguous allocation for new blocks.  
- Log-structured FS; allocate tail of file sequentially.  
- Cache hot tail blocks.  
- Maintain metadata shortcuts (tail-pointer).

---

## 27. Pros & cons of page-table implementations
### a) **Multilevel index**
**Pros:** Saves memory; only allocates needed page-table levels.  
**Cons:** Multiple memory accesses; slower without TLB.

### b) **Hash table**
**Pros:** Fast lookup for sparse address spaces.  
**Cons:** Collisions → slower; poor locality.

### c) **Inverted table**
**Pros:** Small table proportional to physical pages.  
**Cons:** Hash lookup required; slow for shared pages; complex.

Optimizations: larger TLB, clustered hashing, caching of recent translations.

---
