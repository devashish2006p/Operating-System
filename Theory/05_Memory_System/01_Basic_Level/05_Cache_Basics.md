# 1. Cache Memory
Cache is a **small, high-speed, and expensive memory** located close to the CPU.  
It stores the **most frequently used data and instructions**, so the CPU does not need to fetch them from slower RAM.  
Access time is extremely fast — measured in **nanoseconds to picoseconds**.

---

# 2. Purpose of Cache
- Reduce CPU waiting time by storing frequently accessed data.  
- Bridge the speed gap between **fast CPU** and **slower RAM**.  
- Improve overall system performance by minimizing memory access delays.  

---

# 3. Levels of Cache

### L1 Cache
- Located **inside each CPU core**.  
- **Fastest and smallest** cache (32 KB – 128 KB).  
- Stores immediate instructions and data for that core.  

---

### L2 Cache
- Also inside CPU but slightly behind L1.  
- Dedicated **per core**.  
- Larger than L1 (~256 KB – 1 MB) but slower.  
- Acts as a backup for L1 cache misses.  

---

### L3 Cache
- Shared among **all CPU cores**.  
- Located at the **center of CPU chip**.  
- Largest (4 MB – 64 MB) but slowest compared to L1/L2.  
- Helps coordinate data sharing between cores.  

---

### L4 Cache (Optional/Rare)
- Found in some processors on the **motherboard or GPU side**.  
- Provides additional caching for specialized tasks.  
- Rarely used in mainstream CPUs.  

---

# 4. What Cache Stores?

1. **Instructions (Code):** Frequently executed program instructions.  
2. **Data (Operands):** Variables, arrays, and values repeatedly needed by CPU.  
3. **Memory Addresses:** Mapping of recently accessed RAM addresses for quick lookup.  
4. **Decoded Instructions:** Micro-operations stored after decoding in modern CPUs.  
5. **Tag Information:** Identifies which RAM address the cache line belongs to, used for hit/miss detection.  
6. **Metadata (Status Bits):** Valid bit, dirty bit, and replacement policy info to track freshness and synchronization with RAM.  

---

# 5. Characteristics of Cache
- **Speed:** Much faster than RAM, close to CPU clock speed.  
- **Size:** Very small compared to RAM (KBs to MBs).  
- **Cost:** Expensive due to SRAM technology.  
- **Volatility:** Cache is volatile, data lost when power is off.  
- **Hierarchy:** Multiple levels (L1, L2, L3, optional L4) balance speed and size.  

---

# 6. Applications of Cache
- Speeding up CPU instruction execution.  
- Reducing latency in accessing frequently used data.  
- Supporting multi-core processors with shared L3 cache.  
- Improving performance in gaming, multimedia, and heavy computation tasks.  

---
