# 🧩 Memory: The Living Space of Data

Memory is where your computer **stores data while it’s alive and running**.  
When you power off, that data either **vanishes (volatile)** or **stays (non-volatile)** depending on the type of memory.

---

## ⚡ Volatile vs Non-Volatile

|Type|Stays after power off?|Example|Purpose|
|---|---|---|---|
|**Volatile**|❌ No|**RAM** (Random Access Memory)|Temporary workspace|
|**Non-Volatile**|✅ Yes|**ROM**, SSD, HDD|Permanent storage|

Think of **RAM** like your desk — fast and open for quick work.  
Think of **ROM or SSD** like a filing cabinet — slower, but permanent.

---

## 🧠 Static RAM (SRAM)

SRAM is the **race car** of memory: expensive, complex, but lightning fast.

- 1 bit = **1 flip-flop** → made of **6 transistors**
    
- A flip-flop holds its value as long as it’s powered  
    (no refreshing needed)
    
- Access time is extremely fast — measured in **nanoseconds**
    
- Used in **CPU caches (L1, L2, L3)** and inside **SSDs**
    

Because each bit uses so many transistors, SRAM takes more **space and power**,  
so we only use small amounts where **speed matters most**.

> ⚙️ “Constant power, constant readiness.”  
> SRAM is always awake — like a sprinter who never sits down.

---

## 💾 Dynamic RAM (DRAM)

DRAM is the **workhorse** — slower, cheaper, and everywhere.

- 1 bit = **1 capacitor + 1 transistor**
    
- A capacitor stores charge — like a tiny battery that leaks over time
    
- Needs **constant refreshing** to remember its bits
    
- Slower access than SRAM
    
- Found in **main system memory (RAM sticks)**
    

Imagine thousands of little buckets trying not to leak.  
The memory controller runs around **refilling them thousands of times per second**  
so the CPU doesn’t read “empty” bits.

> 🧩 Keywords: _capacitor, refresh, amplifier, cacheable_

---

## 🕓 Asynchronous DRAM (Old School)

In early designs, DRAM didn’t keep pace with the CPU clock.  
It operated **asynchronously** — meaning the CPU would often **wait**  
for memory to respond.

```
RAM  ________|''''''|__|'''|___
CPU  |''''''''|______|'''|___|
         ↑ Delay / wasted cycles
```

Result: Missed cycles, slower throughput, more wasted energy.

---

## 🏎️ Synchronous DRAM (SDRAM)

Then came **SDRAM** — synchronized to the **CPU clock**.  
Now both speak in rhythm, minimizing idle time.

```
RAM  |''''''''|______|'''|___|
CPU  |''''''''|______|'''|___|
```

This harmony made possible faster technologies like **DDR (Double Data Rate)** SDRAM,  
which moves data on both the _rising_ and _falling_ edges of the clock.

> That’s why you see DDR3, DDR4, DDR5 RAM in modern machines — each generation doubles throughput.

---

## 🔬 Anatomy of RAM (Big Picture)

|Type|Cells|Refresh?|Cost|Speed|Typical Use|
|---|---|---|---|---|---|
|**SRAM**|Flip-flops (6T)|❌ No|💰💰💰|⚡⚡⚡|CPU cache|
|**DRAM**|Capacitor + transistor|✅ Yes|💰|⚡|System RAM|
|**SDRAM**|DRAM + sync clock|✅ Yes|💰|⚡⚡|Modern RAM (DDR series)|

---
## Double Data Rate
- DDR SDRAM
- Two transfers Per cycle
- Up and Down
### DDR4 SDRAM
- 64 DATA LINES, 64 pins
- DDR4 prefetch buffer = 8bit io pin
- 