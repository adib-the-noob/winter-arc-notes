## 🧠 The problem: Stack Pointer keeps changing

When a function runs, local variables are stored on the **stack**.

The **stack grows down** in memory:

```
High memory addresses
   ↓
|             |
|    Stack    |
|             |
Low memory addresses
```

---

### 📉 Stack Pointer (SP)

The **stack pointer (SP)** always points to the **top of the stack** —  
but every time you `push` or `pop`, it **moves**.

So, the address keeps changing constantly ⏳

> ❗ That means: if you try to reference a variable using SP, it’s unreliable because SP changes when you push/pop.

---

## 💪 The solution: Base Pointer (BP) (or Frame Pointer)

To make things **stable**, the CPU uses another register:  
→ **Base Pointer (BP)** (on x86, it’s called `ebp` or `rbp`)

When a function starts (e.g. `main`),  
it **stores** the old base pointer and sets a **new one** — fixed for that function’s “stack frame”.

---

### 📦 Stack Frame Visualization

```text
Address     Contents
---------   ------------------------
0xFF9999C   Return address (after call)
0xFF99998   Old Base Pointer (caller’s BP)
0xFF99994   int a   <-- bp - 0
0xFF99990   int b   <-- bp - 4
0xFF998C   int c   <-- bp - 8
0xFF9988   ...     (more local vars)
```

So if we draw this simpler:

```text
bp --> +--------------------+
       | old base pointer   | <- saved from previous function
       +--------------------+
       | return address     |
       +--------------------+
       | int a (bp - 0)     |
       +--------------------+
       | int b (bp - 4)     |
       +--------------------+
       | int c (bp - 8)     |
       +--------------------+
sp --> | ... stack grows ↓  |
```

---

## 🧩 Why “Base” Pointer?

Because it acts like the **base of your stack frame** —  
a _fixed reference point_ to access all local variables and function parameters.

---

### ✅ Example

If you want to access variable `b`:

```
mov eax, [bp - 4]   ; load b
```

If you want to access function parameter 1 (which is above bp):

```
mov eax, [bp + 8]   ; first argument
```

---

## 🧭 Analogy

Imagine a **stack of plates** 🍽️

- The **SP** is your **hand** — it always points to the **top plate**, but moves as you add/remove plates.
    
- The **BP** is like a **sticky note** you put halfway down the stack, saying:  
    “This is where my function’s stuff begins.”  
    You never move that sticky note until the function ends.
    

---

## ⚙️ Nested Calls: Scenario

We have two functions:

```c
int func1(int x, int y) {
    int p, q;
    // some work
}

int main() {
    int a, b, c;
    func1(5, 10);
}
```

---

## 🧱 Step 1 — main() runs first

When `main()` starts, it creates its **own stack frame**:

```text
Address   Contents
-------   -----------------
1024      int a
1020      int b
1018      int c
1014      (old bp)
bp ->     base pointer for main
sp ->     (points below last local var)
```

🧠 **BP (base pointer)** for `main` = `1014`  
**SP (stack pointer)** points to bottom of main’s local variables.

---

## 🧩 Step 2 — main calls func1()

When `main` calls `func1(5,10)`,  
the CPU performs these things **automatically** during a CALL:

1. **Pushes return address** (where to come back after func1 finishes)
    
2. **Pushes old base pointer (bp)** — to remember main’s stack frame
    
3. **Allocates func1’s local variables**
    
4. **Sets bp = sp** (new base pointer for func1)
    

Now func1’s stack frame looks like this 👇

---

## 🧭 Stack During func1()

```text
Address   Contents
-------   ---------------------------
1024      int a           (main)
1020      int b           (main)
1018      int c           (main)
1014      old bp (main)   <-- saved base pointer of main
1010      return address  <-- where to go back after func1
100C      int x (param)
1008      int y (param)
1004      int p (local var)
1000      int q (local var)
bp -> 1014 (now new base for func1)
sp -> 1000 (stack grows downward)
```

---

## 🔁 Step 3 — Nested Call (if func1 calls func2)

If `func1()` calls another function `func2()`,  
the **same pattern repeats:**

- Push return address
    
- Push old BP (func1’s BP)
    
- Set up func2’s new BP
    
- Allocate its locals
    

So now the stack has **multiple layers** —  
each layer (frame) belonging to a **function currently running**.

---

### 🧩 Stack after nested call (simplified view)

```text
|------------------|
| func2 locals     | ← top (SP)
| func2 old BP     |
| func2 return addr|
| func1 locals     |
| func1 old BP     |
| func1 return addr|
| main locals      |
|------------------|
```

Each function has its **own BP**, so it knows:

- Where its local variables are (below BP)
    
- Where its parameters are (above BP)
    
- Where to return (return address)
    
- Where the previous frame is (old BP)
    

---

## 🧠 Analogy

Imagine a **stack of notebooks** 📚 —  
each notebook is one function’s workspace.

- Before starting a new notebook (`func1`), you **bookmark** your old one (push old BP).
    
- You open a new one on top (set new BP, new locals).
    
- When you finish, you **close it** (pop stack, restore old BP) and return to where you left off.
    

---

## 🧹 Step 4 — func1 returns

When `func1` finishes:

1. Deallocate locals → SP moves back up
    
2. Restore old BP (from stack)
    
3. Jump to return address
    

Now you’re back in `main()` — exactly where you paused.

---

Would you like me to **draw the stack visually (like boxes with arrows)** showing how `main → func1 → func2 → return` changes `bp` and `sp` step-by-step?  
That diagram will lock this concept in permanently 🔒