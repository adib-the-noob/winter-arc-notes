Every computer system is built from **resources** — CPU, memory, storage, and I/O devices — managed by the **Operating System (OS)**.  
At the heart of the OS lies the **kernel**, the core component responsible for controlling all system resources, managing processes, and providing the foundation for system calls and device drivers.

---
## CPU (Central Processing Unit)

The CPU is the **brain** of the system — where all computations are executed. Modern CPUs are complex beasts designed for **speed, efficiency, and concurrency**.

### Core Concepts

- **Locality and Cache Hierarchy:**  
    The closer data is to the CPU, the faster it can be accessed.
    
    - **L1 Cache:** Ultra-fast, very small (measured in KBs).
        
    - **L2 & L3 Cache:** Larger but slightly slower.
        
    - **RAM:** Much larger, but significantly slower compared to CPU cache.  
        The OS and hardware work together to keep frequently used data close to the CPU to minimize latency.
    
- **Multiple Cores:**  
    Older CPUs had a **single core**, meaning one process could execute at a time — others waited their turn.  
    Modern CPUs have **multiple cores**, allowing true **parallel execution** of multiple processes or threads.  
    The OS **scheduler** assigns processes to different cores to maximize efficiency.  
    However, this introduces challenges — coordinating multiple cores, managing shared memory, and preventing race conditions.
    
- **Clock Speed:**  
    Each core operates at a certain **frequency (GHz)** — determining how many instructions it can process per second.  
    Higher clock speed generally means faster performance, but also higher power consumption and heat.
    
- **Instruction Set Architecture (ISA):**  
    The CPU understands **machine-level instructions** — binary operations that tell it exactly what to do.  
    All software eventually gets translated to these low-level instructions.
    
- **ARM vs x86 (RISC vs CISC):**
    
    - **ARM (RISC — Reduced Instruction Set Computer):**  
        Uses a smaller, more efficient set of instructions.  
        It’s lightweight, power-efficient, and dominates mobile and embedded systems — also used in Apple’s M-series chips.
        
    - **x86 (CISC — Complex Instruction Set Computer):**  
        Uses a richer, more complex instruction set. It’s powerful for desktops and servers but consumes more power.  
        ARM’s rise in popularity is due to its **performance-per-watt advantage** — a perfect fit for modern efficiency-driven computing.

---
## Memory (RAM)

Memory is the **workspace** where active processes live. It’s incredibly fast compared to storage drives, but much slower than CPU caches — and, importantly, **volatile** (data is lost when power is off).

### Key Ideas

- **Random Access:**  
    RAM stands for **Random Access Memory**, meaning any memory location can be accessed directly — not sequentially like a hard drive or tape.
    
- **Volatility:**  
    Once power is lost, everything in RAM disappears. That’s why programs and data must be saved to persistent storage.
    
- **Used for Process Data & State:**  
    The OS stores each process’s **working data, variables, and execution state** in memory.  
    When memory runs low, the OS might **pause processes**, **swap data to disk**, and **resume execution later** — all invisible to the user.
    
- **Memory Trips are Expensive:**  
    Accessing main memory (RAM) is far slower than accessing CPU cache.  
    Each “trip” to memory — like a frontend calling a backend repeatedly — adds latency.  
    That’s why efficient programs minimize unnecessary memory access.
    
- **Resource Limitations:**  
    RAM is limited. Once it’s exhausted, the system either swaps memory pages to disk (using virtual memory) or kills processes to reclaim space.
    
- **Integration Example — Apple’s Unified Memory:**  
    Apple’s M-series chips combine CPU, GPU, and memory into a **unified architecture**.  
    This design reduces the overhead of moving data between components, improving performance and efficiency — a key advantage of building both hardware and software in-house.

---
## Summary of CPU and Memory

- The **CPU** executes instructions and performs computations; **memory** holds data and program state.
    
- **Caches** bridge the speed gap between the CPU and RAM.
    
- **The kernel** orchestrates all communication and resource management.
    
- **ARM architectures** represent the shift toward energy-efficient, mobile-first computing.
    
- **Understanding system architecture** is foundational — it helps engineers write faster, more efficient, and hardware-aware code.

---

## Storage

Storage is the **persistent layer** of the system — where data survives reboots and power loss.  
Unlike memory (RAM), which is volatile and temporary, **storage devices** hold data long-term.

### Types of Storage

- **HDD (Hard Disk Drive):**  
    Traditional storage that relies on **spinning magnetic platters** and a **read/write head**.  
    It’s mechanical, slower, and prone to physical wear, but offers large capacities at low cost.
    
- **SSD (Solid State Drive):**  
    Modern, **flash-based** storage — no moving parts, significantly faster, and more power efficient.  
    SSDs store data in **NAND flash memory cells**, organized into **pages** (smallest writable  unit) and **blocks** (groups of pages).

---
### SSD Architecture & Behavior

SSDs behave very differently from traditional drives, and understanding this matters if you care about performance or system design.

- **Program–Erase Cycle:**  
    Flash cells can’t overwrite existing data directly.  
    To change a value, an SSD must:
    
    1. **Invalidate** the old data.
        
    2. **Write** new data to a **fresh page**.
        
    3. **Mark** the old page as stale.  
        This is known as **write amplification**, and it’s why SSDs have a finite lifespan — each cell can endure only a limited number of program–erase cycles.
    
- **Whole-Page Writes:**  
    Data can only be written in **full pages**, not in small arbitrary bytes.  
    This means small random writes can trigger large background operations — the SSD has to copy, modify, and re-write whole blocks to preserve consistency.
    
- **Garbage Collection:**  
    Over time, the SSD reclaims invalid (stale) pages through **garbage collection**, moving valid data around and freeing up blocks for new writes.
    
- **Wear Leveling:**  
    To avoid wearing out the same memory cells repeatedly, the SSD’s controller spreads writes evenly across all cells.  
    This extends drive life and maintains performance.
	
	---
	
	### SSD Controllers & OS Interaction
	
	- The **SSD controller** is the drive’s internal brain.  
	    It handles all low-level flash operations — page writes, erases, wear leveling, and garbage collection.  
	    It then exposes a **standardized API (like NVMe, SATA)** to the **Operating System**.
	    
	- The **OS**, in turn, provides higher-level APIs (filesystems, system calls, I/O interfaces) for applications to interact with storage — creating a layered abstraction:    
	
	  ```
	   Application → OS API → SSD Controller API → Flash Memory
	    ```
	
	- This layering can lead to **API overload** — multiple levels of abstraction handling the same I/O operations, each with its own scheduling and caching logic.  
	    High-performance systems often try to **bypass parts of this stack** (e.g., direct I/O, kernel bypass) to reduce latency.
	    
	
	---
	
	### Performance Hierarchy
	
	When comparing speed and latency:
	
	```
	CPU Cache  >  RAM  >  SSD  >  HDD
	```
	
	Each level is slower but larger and cheaper than the previous one.  
	This **memory-storage hierarchy** is fundamental to how systems balance speed, capacity, and cost.
	
	---
	
	### Summary
	
	- **Storage** provides long-term persistence for data.
	- **SSDs** dominate modern systems due to speed and reliability, but they come with **write and erase constraints**.
	- **Controllers** manage flash memory’s physical quirks and expose clean interfaces to the OS.
	- Every read, write, and delete request flows through a multi-layered path — from app to OS to hardware.
	- Understanding how storage works helps engineers design **efficient data systems**, reduce **I/O bottlenecks**, and extend **device lifespan**.
	- 
---

## Networks
- NIC -> Network Interface controller
- Communicate with Other Hosts
- Protocol Implements
## Kernel
- The Core of OS
- Fundamentals of OS == Fundamentals of Kernel
- Manages the Resources
- THe OS is more than the Kernel
	- GUI, Command Line, Ui and Tooling
	- Exmple: Top is a command that extracts processes
	- Distros is all about Tooling
	- 