# 🧩 Heap and Pointers

The **heap** and **pointers** are central to **dynamic memory management** in programs.

---

## 💾 Heap

The heap is a region of memory used to store **large or dynamic data** that:

- **Remains allocated** until you explicitly remove it
    
- **Can be accessed by all functions** if you have a pointer
    
- **Grows upward** in memory (from low → high addresses)
    
- **Allocated/Deallocated** with functions like:
    
    ```c
    malloc()  // allocate
    free()    // deallocate
    new/delete // in C++
    ```
    

---

### 🧠 Analogy: Heap = Warehouse

- Stack = your desk (short-term workspace)
    
- Heap = **warehouse** where you store boxes that you don’t know the size of in advance
    

> You place a box (data) in the warehouse (heap) and keep a **label** (pointer) to it.  
> It stays there until you **remove it explicitly**.

---

## 🧩 Pointer

A **pointer** is a **variable that stores a memory address**.

- Can point to data in:
    
    - **Stack** (local variables)
        
    - **Data section** (global/static variables)
        
    - **Heap** (dynamically allocated memory)
        
- Always stores the **address of the first byte** of the target object
    

### 🔑 Key Points

|Feature|Description|
|---|---|
|Stores|Memory **address** of another variable|
|Types|Pointer to int, char, struct, function, etc.|
|Location|Can live in **stack**, **data section**, or **heap** itself|
|Usage|Indirect access → `*ptr` dereferences it|

---

### 📦 Memory Example

```text
Stack        : int a, int b          <- local variables
Heap         : malloc(100)           <- large data, dynamic
Data section : static/global vars
Pointer      : ptr -> heap or stack
```

---

### 🧠 Analogy: Pointer = Street Address

- Heap = **house**
    
- Pointer = **mailing address**
    
- Dereferencing (`*ptr`) = **enter the house and read the contents**
    

> You can move the pointer anywhere, but the actual data in the heap stays until you explicitly free it.
# 🧩 Memory Leaks

A **memory leak** happens when **heap memory is allocated but never freed**, causing your program to **lose access to it**.

Over time, memory leaks can **slow down or crash** a program because the heap fills up with “orphaned” data.

---

## ⚡ Key Points

|Concept|Explanation|
|---|---|
|**Cause**|Heap memory is **not freed** after use|
|**Lost Pointer**|If a pointer that references heap memory goes out of scope (e.g., function returns), you **can’t free that memory anymore**|
|**Effect**|Memory still occupied → program uses more RAM → possible slowdown or crash|
|**Solution**|Explicitly free memory (`free()` in C/C++) or use **garbage collection / reference counting**|

---

### 🧠 Analogy: Heap = Warehouse, Pointer = Address

- You store boxes in a warehouse (heap).
    
- Pointer = label with the location of your box.
    
- If the pointer (label) is **lost**, the box is still in the warehouse, but you **can’t find it anymore**.
    
- That’s a **memory leak** — space is wasted forever until the program ends.
    

---

### 🔑 Example in C

```c
void func() {
    int* arr = (int*)malloc(100 * sizeof(int));
    // do something
} // arr goes out of scope → memory leak because we never free(arr)
```

**Fix:**

```c
void func() {
    int* arr = (int*)malloc(100 * sizeof(int));
    // do something
    free(arr); // now memory is properly released
}
```

---

### 🧩 Solutions / Prevention

1. **Manual free** — carefully call `free()` when done
    
2. **Reference counting** — track how many pointers reference the memory; free when zero
    
3. **Garbage collection** — automatic memory management (Java, Python, Go)
    

---

Memory leaks are a **classic source of bugs in C/C++**.  
Even small leaks repeated over time can **bring down servers or long-running processes**.

---
# Performance: Stack Vs Heap
- Stack has Builtin memory management
- 