#important-concept

This is one of those core CS fundamentals that most devs skip, yet it’s what separates “I can code” from “I understand what my machine is doing.”  
Here’s your cleaned-up and expanded version with some real-world clarity and depth added.

---

# 🧠 Programs, Linking & Executables

A **program** is the _compiled and linked_ result of source code — transformed into **machine instructions** that a specific CPU architecture can execute.

---

## ⚙️ From Source Code → Executable

### 1. **Compilation**

- Each `.c`, `.cpp`, `.rs`, `.go`, or `.swift` file is **compiled** into an **object file (`.o`)**.
    
- These object files contain:
    
    - Machine-level instructions
        
    - Symbol tables (function and variable names)
        
    - Metadata for linking
        
- Object files are **CPU-architecture specific** — x86, ARM, RISC-V, etc.
    

> Example: compiling for ARM won’t run on x86 — instruction sets differ.

---

### 2. **Linking**

The **linker** takes all object files and combines them into one **executable binary**.

#### 🧩 Static Linking

- All dependent libraries (functions, symbols) are **copied directly** into the executable.
    
- Produces a **large, self-contained binary**.
    
- No runtime dependencies — portable, but heavy.
    
- Used in embedded systems or Go binaries.
    

#### 🔗 Dynamic Linking

- Instead of copying libraries, the linker stores **pointers/references** to external libraries.
    
- At runtime, the **loader** finds and links these libraries in memory.
    
- Much smaller binaries, but if the required library (`.dll`, `.so`, `.dylib`) is missing →  
    ❌ “**Dynamic Library Not Found**” error.
    

> Example:
> 
> - **Windows:** `.dll` (Dynamic Link Library)
>     
> - **Linux:** `.so` (Shared Object)
>     
> - **macOS:** `.dylib`
>     

---

### 3. **Executable Formats**

Once linked, the final binary is written in a specific **executable file format** understood by the OS and loader:

|OS|Executable Format|Example|
|---|---|---|
|Linux|ELF (Executable and Linkable Format)|`/bin/bash`|
|macOS|Mach-O|`/usr/bin/ls`|
|Windows|PE (Portable Executable)|`C:\Windows\System32\cmd.exe`|

Each format defines:

- How code, data, and symbols are laid out in memory
    
- Where the **entry point** (`main()`) is
    
- How to perform dynamic linking

---

### 4. **CPU Architecture Dependency**

Executables are **tied to their CPU instruction set**:

- x86, x86_64, ARM, RISC-V — all have different opcodes
    
- Apple’s M1/M2/M3 chips (ARM-based) require binaries built for ARM
    
- That’s why Apple uses **Rosetta** to emulate x86 apps on ARM Macs

> “Compile once, run anywhere” only works if you’re compiling to **a common intermediate layer** (like JVM bytecode or WebAssembly).

---

### 5. **At Rest**

When not running:

- The program **lives on disk** as a static executable file.
    
- It becomes **a process** only when loaded into memory by the OS loader.
    

---

## 🧩 Recap

|Stage|Purpose|Output|
|---|---|---|
|Compile|Turn source → object code|`.o` files|
|Link|Combine objects + libraries|`.exe`, ELF, etc.|
|Load|OS loads binary to memory|Running **process**|

---

### ⚡ Summary

- **Program**: compiled + linked machine code for a specific CPU
    
- **Linker**: glues together all compiled parts
    
- **Static linking**: everything baked into the binary
    
- **Dynamic linking**: shared libraries loaded at runtime
    
- **Executable format**: defines how OS loads and executes code
    
- **Lives on disk** → becomes a **process in memory** when run

---
# ⚙️ From Program → Process

When a **program** is executed, it transforms from a _file on disk_ into a **process** — a living, breathing entity in system memory.  
This is where operating systems earn their keep: managing who runs, when, and where.

---

## 🧠 What Is a Process?

A **process** is an _instance_ of a running program — a private, isolated world containing everything that program needs to execute.

The moment you run a command like:

```bash
./myapp
```

the OS **loader** performs a careful ritual:

1. Reads the program’s executable format (ELF, PE, Mach-O).
    
2. Allocates memory regions for **code**, **stack**, **heap**, and **data**.
    
3. Initializes essential registers (like the **instruction pointer**, which tells the CPU what to execute next).
    
4. Creates a **Process Control Block (PCB)** — a detailed record the OS uses to track and manage that process.
    

---

## 🧩 Process Control Block (PCB)

The **PCB** is the OS’s cheat sheet for each process.  
It holds all the metadata needed to pause, resume, or terminate it. Think of it as the “save file” of your program’s life.

A typical PCB contains:

- **Process ID (PID)** – unique identifier.
    
- **Program Counter (Instruction Pointer)** – address of the next instruction to execute.
    
- **CPU Registers** – current working state.
    
- **Memory Pointers** – references to code, stack, and heap.
    
- **Process State** – running, ready, waiting, or terminated.
    
- **Accounting Info** – CPU time used, priority, owner.
    
- **I/O Info** – open files, sockets, and devices.
    

Whenever the OS switches tasks (context switch), it saves this PCB data — then restores it later to resume execution exactly where it left off.

---

## 🧱 Memory Layout of a Process

When loaded into memory, a process is divided into well-defined regions:

```
+---------------------------+
| Stack (local variables)   | ← grows downward
+---------------------------+
| Heap (dynamic memory)     | ← grows upward
+---------------------------+
| Data Segment (globals)    |
+---------------------------+
| Text Segment (code)       |
+---------------------------+
```

**Stack:**  
Temporary, function-level memory. Stores local variables, function call frames, and return addresses. Managed automatically — expands and contracts as functions are called and return.

**Heap:**  
Dynamic memory region used for allocations via `malloc()`, `new`, etc. Managed manually (or by garbage collectors). Grows upward during runtime.

Together, these give each process its _own private address space_ — isolation that prevents one process from tampering with another’s memory.

---
## 🧭 Instruction Pointer (Program Counter)

At the CPU level, the **Instruction Pointer (IP)** (called **PC** in some architectures) marks _the address of the next instruction to execute_.  
After each instruction, it moves forward — unless a jump, branch, or function call modifies it.

When you debug and “step through code,” you’re literally watching the instruction pointer march through your program’s memory.

---
## 🧩 When Processes Multiply

Every process begins life as a **child** of another — typically created by a `fork()` system call. 
On Linux, the original ancestor is `init` or `systemd`, the very first process after boot.
Through `fork()` + `exec()`, your shell spawns new processes for every command you run.

> Example: When you type `python3`, your shell forks itself, then replaces the child process’s memory with the Python interpreter binary using `exec()`.

---
## 💾 Process Lifecycle

A process moves through a few key states during its existence:

1. **New** – being created.
    
2. **Ready** – waiting in line for CPU time.
    
3. **Running** – currently executing instructions.
    
4. **Waiting/Blocked** – paused, waiting for I/O or event.
    
5. **Terminated** – completed or killed; PCB is removed.
    

The OS scheduler orchestrates this dance, deciding which process gets CPU time next.

---

## ⚡ Summary

- **Program** → static file on disk.
    
- **Process** → program _in motion_, living in memory.
    
- **PCB** → the OS’s tracking sheet for each running process.
    
- **Stack/Heap** → temporary and dynamic memory regions within each process.
    
- **Instruction Pointer** → the CPU’s current “where am I?” marker.
    
- **PID** → how the OS (and you) refer to a specific process.
    

---
![[Pasted image 20251008025208.png]]

Exactly right, Adib ✅  
You’ve got the main idea perfectly.
Here’s what happens step-by-step:

1. **You type `python3` in your shell.**  
   The shell parses the command line and finds where the `python3` program lives (e.g., `/usr/bin/python3`).

2. **The shell calls `fork()`.**  
   This **duplicates itself** — now there are *two* almost identical processes:
   - The **parent**: still your shell (waiting for the command to finish).  
   - The **child**: a new process that’s an *exact copy* of the shell at that moment.

3. **The child calls `exec("python3", ...)`.**  
   The exec system call **replaces** the child’s memory with the Python interpreter program.  
   So now, that child process is no longer a shell — it’s a brand-new Python process with its own code and memory image.

4. **They run separately.**  
   - The parent shell process stays alive, waiting for the Python process to finish.
   - The child (now Python) runs independently.

5. **When Python exits**, the shell gets notified (through a system call like `wait()`) and prints a new prompt — ready for the next command.

So yes — your summary is correct:  

> When you type `python3`, the shell replicates itself using `fork()`, then the copy calls `exec(python3)`, becoming a separate process.

That’s precisely how command execution works in Unix-like systems 💡

# Shell

The **shell** is a user‑level program that acts as an interface between you and the operating system’s **kernel** — it interprets the commands you type, translates them into system calls (like fork(), exec(), cd, etc.), and manages how programs are launched, connected, and controlled. In simple terms, it’s your **command interpreter**: instead of you talking directly to the kernel through raw system calls, you talk to the shell, and the shell talks to the kernel to perform actions like running processes, handling input/output redirection, or managing jobs.