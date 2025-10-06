#cs/fundametals #favourites
## OS Memory Space, Drivers & System Calls

## Memory Spaces

Modern operating systems **split memory** into two main regions:

### 🧑‍💻 User Space

Where **user applications** run — isolated from the kernel for stability and security.

Examples:

- Chrome / Firefox (Browser)
    
- Postgres, MySQL
    
- Python, Node.js, etc.

Characteristics:

- Each process gets its own **virtual address space** (private memory).
    
- Cannot directly access hardware or kernel memory.
    
- Communicates with the kernel **only via system calls**.
    
- If it crashes, it doesn’t crash the OS.

---

### 🛡️ Kernel Space

Where **core OS logic and drivers** live.

Includes:

- **Kernel Code:** process management, memory, I/O, scheduling
    
- **Device Drivers:** software interface to physical hardware
    
- **TCP/IP Stack:** handles networking at the system level
    
- **Filesystem Handlers, Syscall Table, Interrupt Handlers**

The kernel runs in **privileged mode** (ring 0), giving it full access to the CPU and hardware.  
User programs run in **unprivileged mode** (ring 3).

---

### ⚡ Isolation Boundary

The **user–kernel boundary** is strictly enforced — a process cannot access kernel memory directly.

This isolation:

- Prevents malicious apps from crashing the OS
    
- Maintains system stability
    
- Adds performance cost due to context switching

However, newer technologies like **`io_uring`** in Linux optimize around that:

- Allows asynchronous I/O without full mode-switch overhead
    
- Submits I/O requests via shared memory ring buffers
    
- Essentially **“bends” the isolation rule for performance**

---

## Device Drivers

Drivers are **kernel modules** that act as translators between hardware and software.

Examples:

- **NVMe Driver:** manages SSD read/write queues
    
- **NIC Driver:** manages network packets
    
- **Keyboard / Mouse Driver:** interprets input events
    

Responsibilities:

- Handle **hardware interrupts** (signals from devices)
    
- Implement **Interrupt Service Routines (ISRs)** to respond fast
    
- Provide a **device file interface** (e.g., `/dev/sda`, `/dev/nvme0n1`) so user apps can interact with hardware via system calls
    

In short:

> Drivers make hardware look like a file or resource that software can read/write.

---

## System Calls (Syscalls)

User programs **can’t directly call** kernel functions.  
They make **system calls** — controlled entry points into kernel mode.

Examples:

```c
read()
write()
open()
close()
socket()
mmap()
```

### How it works:

1. User code calls `read(fd, buf, size)`.
    
2. The CPU triggers a **mode switch** (user → kernel).
    
3. The kernel executes the requested operation (e.g., disk I/O).
    
4. It switches back (kernel → user) and returns the result.
    

This transition is costly because:

- CPU context switches to privileged mode.
    
- Cache/TLB may be invalidated.
    
- Kernel and user stacks differ.
    

Hence the motivation for **`io_uring`, eBPF, and user-space networking stacks** — they reduce syscall overhead.

---

## Quick Recap

|Concept|Runs In|Purpose|
|---|---|---|
|**User Space**|Ring 3|Apps like Chrome, Postgres, etc.|
|**Kernel Space**|Ring 0|Core OS + Drivers + TCP/IP|
|**Device Driver**|Kernel|Interface to hardware|
|**System Call**|Transition between user ↔ kernel|Controlled access to hardware & OS functions|
![[Screenshot From 2025-10-06 22-40-45.png]]