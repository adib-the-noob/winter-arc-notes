# ⚙️ From C Code → Compilation → Machine Code → Process

Everything a computer does starts with this chain of transformations — turning readable logic into raw CPU instructions, and finally, into a living process.

---

## 🧩 Step 1: Source Code (C Language)

You start with human-readable logic:

```c
int main() {
	int a = 1;
	int b = 2;
	int c = a + b;
	printf("a + b = %d", c);
	return 0;
}
```

Here’s what’s happening conceptually:

- `int a = 1;` → allocate a small chunk of memory for variable `a`, store value `1`
    
- `int b = 2;` → allocate memory for `b`, store value `2`
    
- `int c = a + b;` → read both values, add them, store in `c`
    
- `printf(...)` → call a library function to print to standard output (stdout)
    

At this point, it’s pure **logic** — the compiler hasn’t yet turned it into CPU-understandable instructions.

---

## ⚙️ Step 2: Compilation → Assembly

When you compile this (`gcc main.c -o main`), the compiler translates the C into **assembly code** — symbolic shorthand for the machine’s native instruction set.

Example (simplified ARM-style pseudo-assembly):

```asm
mov r0, #1        ; a = 1
mov r1, #2        ; b = 2
add r2, r0, r1    ; c = a + b
bl printf         ; branch to library function
```

Each instruction corresponds directly to an operation the CPU can perform — load, add, branch, store.

---

## ⚡ Step 3: Assembly → Machine Code

The assembler converts each symbolic instruction into a **binary encoding** — sequences of 1s and 0s representing _operation codes (opcodes)_ and _operands_.

```text
0x11223344  mov r0, #1
0x11223345  mov r1, #2
0x11223346  add r2, r0, r1
0x11223347  bl printf
```

Those hexadecimal numbers aren’t arbitrary — they’re the machine’s _vocabulary_.  
When the CPU sees `0x11223346`, it doesn’t “understand” addition the way humans do — it simply knows that pattern means “add the contents of register r0 and r1, and put the result in r2.”

This machine code is stored in your executable file — the **text segment** of your program.

---

## 🧠 Step 4: From Executable to Running Process

Now when you run the binary (`./main`):

1. The OS **creates a process**, assigning it a unique **PID (process ID)**.
    
2. It **loads** the binary’s machine instructions into memory.
    
3. Initializes stack, heap, registers, and the **instruction pointer** (pointing to `main()`).
    
4. Starts executing instructions line by line, updating registers and memory as it goes.
    

So, your CPU literally begins moving electrical states through transistors following the encoded sequence `mov → mov → add → bl`.

---

## 🧩 Step 5: Multi-Process Example — `postgres.exe`

On your system, you might see:

```
postgres.exe → PID 1
postgres.exe → PID 2
postgres.exe → PID 3
```

Each is a **separate process** — an independent copy of the same executable, each with its _own memory space_, _stack_, and _registers_, but sharing the same _underlying code_ on disk.

PostgreSQL runs this way because it uses a **process-per-connection model**:  
the main server (PID 1) forks child processes (PID 2, PID 3, …) to handle individual client connections.

So while the binary (`postgres.exe`) is one file, the OS creates multiple **process instances** from it — each running the same machine code but living in separate memory universes.

---

## ⚡ Summary

- **C code**: human-readable logic
    
- **Assembly**: symbolic CPU instructions
    
- **Machine code**: binary encoding the CPU executes
    
- **Executable**: machine code + metadata stored on disk
    
- **Process**: that executable loaded, running, and alive in memory
    
- **Multiple PIDs**: multiple independent instances of the same program
    

---

When you look at it that way, “running a program” is really the art of convincing a lump of silicon to carry out your abstract thought — one instruction at a time.