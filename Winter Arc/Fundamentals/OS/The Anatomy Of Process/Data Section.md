# 🧩 Data Section in Memory

The **Data Section** is a part of a program’s memory layout that stores **global and static variables**.

It’s like a **special shelf** in memory: fixed, known, and accessible by all functions in your program.

---

## ⚡ Key Characteristics

| Feature         | Description                                             |
| --------------- | ------------------------------------------------------- |
| **Purpose**     | Stores **static** and **global variables**              |
| **Access**      | Referenced **directly by memory address**               |
| **Permissions** | Can be **read-only** or **read-write**                  |
| **Scope**       | All functions in the program can access these variables |
| **Size**        | **Fixed at compile time** (like the code/text section)  |

---

### 🧠 Analogy

Think of the **data section** as a **library shelf**:

- Each book = a global/static variable
    
- The shelf has **fixed space** allocated at the start
    
- Any function in the program can **walk over and grab a book**
    
- Some books are **locked for reading only** (read-only), others you can **write notes in** (read-write)
    

---

### 📦 Memory Layout Context

In a typical program:

```
+-----------------+ High Memory
|    Stack        |  grows downward
+-----------------+
|    Heap         |  grows upward
+-----------------+
|   BSS Section   |  uninitialized globals/static
+-----------------+
|   Data Section  |  initialized globals/static
+-----------------+
|   Code/Text     |  instructions
+-----------------+ Low Memory
```

- **Data Section** stores **initialized global/static variables**
    
- **BSS Section** stores **uninitialized global/static variables**
    

---

### 💡 Key Points

- **Fixed memory** → compiler knows addresses ahead of time
    
- Accessible by **all functions**
    
- Can be **read-only** (like `const`) or **read/write**
    
- Unlike stack/heap, it **doesn’t grow/shrink** at runtime