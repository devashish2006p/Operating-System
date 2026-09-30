
# 1. Introduction to Memory Management
Memory Management operating system ka woh process hai jisme OS RAM (main memory) ko manage karta hai, yani processes ko memory allocate karta hai, unke memory usage ko track karta hai aur zaroorat padne par memory free karta hai, taaki multiple processes efficiently execute ho sakein.

## Challanges in Memory Allocation
Memory allocation mein OS ko kai challenges face karne padte hain, kyunki multiple processes ko limited RAM efficiently allocate karni hoti hai.
1. Memory Fragmentation: Memory chhote-chhote unused blocks mein divide ho jaati hai, jisse kabhi-kabhi enough total free memory hone ke baad bhi process ko required memory nahi mil paati.
2. Memory Allocation Efficiency: OS ko decide karna hota hai ki kis process ko kitni memory aur kis location par allocate kare, taaki memory waste na ho.
3. Memory Protection: OS ko ensure karna hota hai ki ek process doosre process ki allocated memory ko bina permission access ya modify na kar sake.
4. Memory Sharing: Multiple processes ko kabhi-kabhi same memory ya data share karna hota hai, lekin sharing ke dauran data consistency aur security maintain karni padti hai.
5. Limited Physical Memory: RAM limited hoti hai, isliye jab multiple processes memory demand karte hain, toh OS ko available memory carefully manage karni padti hai.

## Memory Management Techniques 
1. Contiguous Memory Allocation – Process ko RAM mein ek continuous block of memory diya jaata hai. Ismein fixed aur variable partitioning aate hain.
- **Types**
  1. Single Contiguous Allocation : RAM mein ek portion OS ka liye aur dosra ek user process ka liya hota hai; ek time par generally ek hi user process memory mein hota hai. Agr process user portion sa bada hai to woh us memory mein load nahi ho shakta hai. 
  2. Fixed Partitioning (MFT) : RAM ko pehle sa fixed size partitions mein divide kar diya jata hai aur her partitioin mein ek process load hota hai.
  3. Variable Parttioning (MVT) : Process ek requriement ka accordinig RAM mein dynamically different size partitons banaye jate hai.
- **Advantages**
  1. Simple Management: OS ke liye process ko ek continuous memory block allocate aur manage karna relatively simple hota hai.
  2. Fast Access: Process ke data tak pahunchna efficient ho sakta hai, kyunki memory ek continuous region mein hoti hai.
  3. Low Overhead: Ismein paging jaise page tables maintain karne ka additional overhead nahi hota.
- **Disadvantages**
  1. External Fragmentation: Free memory chhote-chhote blocks mein divide ho sakti hai, jisse large process ko memory dena difficult hota hai.
  2. Difficult Expansion: Agar process ko baad mein zyada memory chahiye, toh uske block ko expand karna difficult ho sakta hai.
  3. Memory Wastage: Fixed partitioning mein process ko allocated partition se kam memory chahiye, toh bachi hui space waste ho sakti hai.

2. Non-Contiguous Memory Allocation – Process ko RAM ke alag-alag locations par memory mil sakti hai. Ismein paging aur segmentation aate hain.
- **Types**
  1. Paging - Paging ek non-contiguous memory management technique hai jisme OS process ki virtual memory ko fixed-size blocks (pages) aur RAM ko same-size blocks (frames) mein divide karta hai, taaki process ke pages RAM ke alag-alag locations par store ho sakein.
  2. Segmentation - Segmentation ek non-contiguous memory management technique hai jisme OS process ki memory ko fixed-size pages ke bajay alag-alag size ke logical parts (segments) mein divide karta hai, jaise Code, Data, Stack aur Heap, aur har segment ko memory mein alag jagah allocate kar sakta hai.
- **Advantages**
  1. Better Memory Utilization: Process ko RAM mein ek continuous block ki zaroorat nahi hoti, isliye alag-alag free locations use ki ja sakti hain.
  2. Flexible Allocation: OS process ki memory ko different physical locations par allocate kar sakta hai.
  3. Reduced External Fragmentation: Paging mein fixed-size frames use hone ki wajah se external fragmentation ki problem largely eliminate ho jaati hai.
- **Disadvantages**
  1. Management Complexity: OS ko memory ke different parts ka mapping aur tracking maintain karna padta hai.
  2. Mapping Overhead: Paging mein page tables maintain karni padti hain, jisse extra memory aur processing overhead hota hai.
  3. Address Translation: Virtual address ko physical address mein translate karne ke liye additional hardware support ki zaroorat hoti hai.

3. Virtual Memory Management – OS virtual address space manage karta hai aur zaroorat ke hisaab se RAM aur secondary storage ka use karta hai. Ismein demand paging jaise mechanisms aate hain.

- **Advantages**
  1. Larger Address Space: Process ko physical RAM se bada virtual address space mil sakta hai, jisse large applications ko manage karna easier hota hai.
  2. Efficient RAM Usage: OS sirf required pages ko RAM mein rakh sakta hai, jisse unnecessary memory usage kam ho sakta hai.
  3. Process Isolation: Har process ka alag virtual address space hota hai, jisse processes ki memory ko isolate aur protect karna easier hota hai.
- **Disadvantages**
  1. Page Fault Overhead: Jab required page RAM mein nahi hota, toh OS ko use load karna pad sakta hai, jisse execution slow ho sakta hai.
  2. Disk I/O Delay: Agar page ko storage se load karna pade, toh RAM ke comparison mein access kaafi slow hota hai.
  3. Thrashing: Agar RAM par bahut zyada memory pressure ho, toh system pages ko baar-baar RAM aur storage ke beech move kar sakta hai, jisse performance bahut degrade ho sakti hai.
 



# 2. Memory Organization and Tracking
## 1. Memory Organization
Memory Organization ka matlab hai RAM ko kis tarah structure aur parts mein arrange kiya jata hai, taaki program aur data ko memory mein sahi locations aur layout ke according store kiya ja sake.
### Types of Memory Organization
1. Physical Memory Organization - Isme hum samajhte hai ki actual RAM ka structure kaisa hota hai, memory cells aur addresses kis tarah arranged hote hai, aur physical memory ma data kis location par store hota hai. 
  - **Memory Cells** : Memory Cell memory ka ek chhota storage element hota hai, jo information store karta hai. Digital computer mein information bits ke form mein hoti hai, isliye ek basic memory cell aam taur par 1 bit (0 ya 1) store karta hai.
      - **Memory Cell ka ander kya hota hai?**
        1. DRAM - Ek bit ko store karne ka liye aam taur par ek transistor aur ek capacitor ka use hota hai.
        2. SRAM - Ek bit ko store karne ka liye aam taur par multiple transistors ka circuit use hota hai. 

---
2. Logical Memory Organization - Ishme hum samajhte hai ki program ke prespective sa memory ka structure kaisa hota hai aur ushki memory ko code, data heap aur stack jaisa regions ma kaisa organize kiya jata hai. 
# 3. Memory Allocation and Deallocation

# 4. Address Translation and Mapping

# 5. Virtual Memory Management 

# 6. Memory Protection and Sharing 

---

# 2. Address Space 
Address Space har process ka apna virtual address-map hota hai, jiske addresses ko use karke program apne code aur data access karta hai. OS aur hardware in virtual addresses ko actual physical memory se map karte hain, jisse program ko apni private memory hone ka illusion milta hai.
- **Components of Address Space**
  1. Code (Text): Is region mein program ke executable instructions store hote hain, jinhe CPU execute karke program ke operations perform karta hai.
  2. Data: Is region mein initialized global aur static variables store hote hain, jinhe program ke start hone se pehle initial values di gayi hoti hain.
  3. BSS: Is region mein uninitialized ya zero-initialized global aur static variables store hote hain, jinhe program ke shuru hone par zero value milti hai.
  4. Heap: Is region mein runtime par dynamically allocated memory hoti hai, jaise C mein malloc() se li gayi memory, jiska size program ke chalne ke dauran badal sakta hai.
  5. Stack: Is region mein function calls se related data, jaise local variables, function arguments aur return information store hoti hai, jiska use function execution manage karne ke liye hota hai.
  6. Memory-mapped regions: Is region mein shared libraries, mapped files aur anonymous memory mappings jaise areas hote hain, jinhe process apne virtual address space mein map karke access kar sakta hai.
 
- **Arrangement of Address Space**
  1. Code(text)
  2. Data
  3. BSS
  4. Heap
  5. Memory Mapped regions
  6. Stack
 
- **Virtual Address Space and Physical Address Space**
    - **Virtual Address:** Address jo program use karta hai apne address space ke andar.
    - **Physical Address:** Actual RAM mein woh location jahan data ya instruction physically stored hai.
 
- **Goal of Memory Visualization**
1. Transparency — program ko virtual memory ka pata na chale.
2. Efficiency — virtualization ki wajah se time aur memory ka unnecessary overhead na ho.
3. Protection — ek process doosre process ya OS ki memory ko unauthorized access ya modify na kar sake.

# 3. Stack & Heap Memory 
  1. Stack - Stack memory ko compiler automatically manage karta hai. Tumhe manually malloc() ya free() use nahi karna padta, jab tum normal local variables declare karte ho.
  2. Heap - Heap memory mein allocation aur deallocation tum explicitly karte ho. Iske liye C mein commonly malloc() aur free() use hote hain. Heap ki memory function return ka baad bhi reh shakti hai qoki allocation ka bad memory ko free v karna hota hai.

  - **malloc()** : malloc() ka use heap par memory allocate karne ke liye hota hai. Tum isko batate ho ki kitne bytes chahiye.
