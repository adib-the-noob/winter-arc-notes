#cs/fundametals 
## Processes & Programs

In any modern operating system, the kernel’s job is to **run and manage processes** — each representing a running instance of some program.

Example:

```
postgres.exe → postgres pid 1, postgres pid 2, postgres pid 3
mysql.bin    → mysql pid 1, mysql pid 2
```

Each **PID (Process ID)** represents an independent instance of the program, with its own memory space, state, and system resources.

---

### Program

- A **program** is a **compiled executable file** — code stored on disk, not yet running.
    
- It becomes a **process** only when it’s loaded into memory and executed by the OS.
    
- Think of a program as a recipe, and a process as the act of cooking it.
    

A program contains:

- **Machine instructions**
    
- **Static data**
    
- **Metadata** (symbols, relocations, headers)
    

---

### Process

- A **process** is a **program in motion** — an executing instance with:
    
    - Its own **address space** (memory layout)
        
    - **Open file descriptors** (handles to files, sockets, etc.)
        
    - **Execution context** (registers, stack, program counter)
        
    - **Process ID (PID)** to identify it uniquely
        

The OS ensures that each process is **isolated** from others for stability and security.

---

### Executable File Formats

Different operating systems have their own binary executable formats — each defining how code, data, and metadata are structured inside a compiled program.

| OS          | Format | Meaning                          |
| ----------- | ------ | -------------------------------- |
| **Linux**   | ELF    | _Executable and Linkable Format_ |
| **macOS**   | Mach-O | _Mach Object File Format_        |
| **Windows** | PE     | _Portable Executable Format_     |

These formats tell the OS **how to load** and **where to execute** the code — which segments go into memory, where dynamic libraries link, and how symbols are resolved.

---

## Process Management

Once a program becomes a process, the **kernel** takes over control of its lifecycle.

### Kernel Responsibilities

- **Process Creation:**  
    The kernel loads the program into memory and creates a process control block (PCB) — a data structure storing its state.
    
- **Scheduling:**  
    The kernel decides **which process gets CPU time**.  
    It schedules processes across cores, balancing performance and fairness.
    
- **Context Switching:**  
    When the CPU switches from one process to another, the kernel **saves the current state** (registers, program counter) and **loads the next process’s state**.  
    This makes multitasking possible — multiple processes appear to run “simultaneously” even on limited cores.
    
- **Resource Management:**  
    The kernel **grants processes access** to system resources:
    
    - **CPU time**
        
    - **Memory and I/O**
        
    - **Storage and network handles**
        
- **Process Termination:**  
    When a process exits, the kernel **cleans up its resources**, closes file handles, and releases memory.
    

---

### Summary

- **Programs** are static executable files on disk.
    
- **Processes** are dynamic instances of those programs, running in memory.
    
- The **kernel** manages all process creation, scheduling, and resource allocation.
    
- Different OSs use different **executable formats** (ELF, PE, Mach-O), but the underlying concept — _a program in motion_ — remains universal.
    

---

If you want, I can follow this note with **“Threads, Context Switching & IPC (Inter-Process Communication)”** — it’s the natural next step after this, explaining how processes interact and how multithreading fits into OS-level scheduling.