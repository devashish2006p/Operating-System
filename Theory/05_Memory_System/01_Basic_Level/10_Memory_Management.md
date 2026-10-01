
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
 

```

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
```
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

# 4. Memory Address Translation
## 1 Memory Virtualization ka Introduction
Gautam, Memory Virtualization ka matlab hai ki OS har process ko aisa illusion deta hai ki uske paas apni private memory hai, jabki actually multiple processes computer ki physical RAM share kar rahe hote hain.

### 1.1 Limited Direct Execution (LDE)
Limited Direct Execution CPU virtualization ka ek mechanism hai. Iska main idea hai ki program ko mostly directly CPU hardware par execute hone diya jaye, taaki execution efficient rahe. Lekin kuch important situations mein OS control leta hai aur ensure karta hai ki system sahi tarike se operate kare.
OS ko control milne ke kuch important occasions hain:
  1. System call: Jab application OS se koi service request karti hai, jaise file access ya process-related operation.
  2. Timer interrupt: Jab timer interrupt generate hota hai, OS CPU ka control lekar scheduling kar sakta hai.
  3. Exception: Jab execution ke dauran koi exceptional condition aati hai, CPU OS ko control de sakta hai.
Is approach mein OS har instruction ke execution mein interfere nahi karta. Woh critical points par intervene karta hai, jisse efficiency aur control dono maintain hote hain.

### 1.2 Memory Virtualization ke Goals
Memory virtualization mein bhi LDE jaisi strategy use hoti hai. Iske teen important goals hain:
  - Efficiency: Memory access fast hona chahiye. Hardware support address translation ko quickly perform karta hai.
  - Control and protection: Application ko sirf apni allowed memory access karne deni chahiye. Ek process doosre process ya OS ki memory ko illegally access nahi karna chahiye.
  - Flexibility: Program ko apne address space ko apni requirement ke according use karne ki freedom milni chahiye.
In goals ko achieve karne ke liye hardware aur OS dono ka cooperation zaroori hai.

## 2. Hardware-Based Address Translation
### 2.1 Definition
Address translation ek hardware mechanism hai jo process ke virtual address ko physical address mein convert karta hai, jahan actual information memory mein located hoti hai.
Process virtual addresses generate karta hai. Hardware unhe translate karke physical memory ki correct location tak pahunchata hai.

### 2.2 Address Translation kaise hota hai?
Har memory reference par hardware address translation perform karta hai. Ismein teen important types of memory access include hote hain:
  1. Instruction fetch: CPU jab next instruction memory se fetch karta hai, to instruction ka address translate hota hai.
  2. Load: Jab instruction memory se data read karke register mein rakhti hai, to data ka address translate hota hai.
  3. Store: Jab instruction register ka data memory mein write karti hai, to destination address translate hota hai.
> Iska matlab address translation sirf data read/write ke liye nahi, instruction fetch ke liye bhi hota hai.

### 2.3 Hardware aur OS ki responsibilities
**Hardware:**
  - Har memory reference par virtual address ko physical address mein translate karta hai.
  - Memory access ko correct physical location par redirect karta hai.
  - Address valid hai ya nahi, check karne ke liye circuitry provide karta hai.
  - Illegal access hone par exception raise karta hai.
**OS:**
  - Hardware ko correct translation settings provide karta hai.
  - Physical memory ke free aur occupied areas track karta hai.
  - Processes ko memory allocate aur reclaim karta hai.
  - Context switch par hardware registers update karta hai.
  - Exceptions ko handle karta hai.
> Hardware low-level mechanism provide karta hai, jabki OS us mechanism ko configure aur manage karta hai.

### 2.4 Memory Virtualization ka Illusion
Memory virtualization ka purpose process ko ye illusion dena hai ki uske paas apni private memory hai.
**Process ko lagta hai:**
  - Uska address space address 0 se start hota hai.
  - Uske code aur data uske apne address space mein hain.
  - Woh apne virtual addresses se memory access kar raha hai.
> Actual physical memory multiple programs ke beech share hoti hai. OS aur hardware milkar virtual addresses ko actual physical locations se connect karte hain.
> Process ko generally pata nahi hota ki uske memory references translate ho rahe hain. Isi ko transparency kehte hain.

### 2.5 Interposition
Interposition ek general technique hai jisme kisi well-defined interface ke beech mein ek mechanism insert kiya jata hai, taaki additional functionality provide ho sake.
  > Memory virtualization mein hardware process ke memory reference aur actual physical memory access ke beech interpose karta hai.
  > Process virtual address generate karta hai, hardware us address ko translate karta hai, aur phir memory system actual location access karta hai.
  > Interposition ka ek important benefit transparency hai, kyunki client ko apna interface change nahi karna padta.

## 3. Dynamic (Hardware-Based) Relocation
### 3.1 Definition
Dynamic relocation ek hardware-based address translation technique hai jisme process ke virtual addresses ko runtime par physical addresses mein convert kiya jata hai. Is technique ko Base-and-Bounds bhi kehte hain, kyunki ismein base register aur bounds register ka use hota hai.

### 3.2 Base Register
Base register mein physical memory ka starting address store hota hai jahan process ka address space load kiya gaya hai.
Example:
  - Process physical memory mein 32 KB se start hota hai.
  - Base register ki value 32 KB hoti hai.
> Base register virtual address ko physical memory ki correct location tak relocate karne mein help karta hai.

### 6.3 Bounds Register
Bounds register mein process ke address space ka size store hota hai.
Example:
  - Process ka address space size 16 KB hai.
  - Bounds register ki value 16 KB hogi.
  - Valid virtual addresses 0 se 16 KB - 1 tak honge.
> Bounds register ka purpose ye ensure karna hai ki process apni allowed virtual address range ke andar hi memory access kare.
> Agar virtual address bounds ke equal ya usse bada ho, ya negative ho, to CPU exception raise karta hai.
### 6.4 Address Translation Formula
> Physical Address = Virtual Address + Base 
> Hardware pehle bounds check karta hai. Address valid hone par base add karke physical address generate karta hai.

### 6.5 Bounds Checking aur Protection
Bounds register process ko apni allowed memory ke bahar access karne se rokta hai.
```
Maan lo bounds 16 KB hai:
Virtual address	Check	Result
10 KB	10 KB < 16 KB	Valid
15 KB	15 KB < 16 KB	Valid
16 KB	16 KB bounds ke equal hai	Fault
18 KB	18 KB > 16 KB	Fault
Negative address	Allowed range se bahar	Fault
Invalid address par CPU exception raise karta hai aur OS handler ko control deta hai.
Bounds checking address translation ke saath memory protection provide karta hai.
```
### 6.6 Static Relocation
Static relocation software-based relocation technique hai jisme loader executable ke addresses ko run hone se pehle rewrite karta hai.
Example:
  - Program ek instruction mein address 1000 use karta hai.
  - Loader process ko physical memory ke address 3000 se load karne ka decide karta hai.
  - Loader instruction mein address ko 1000 + 3000 = 4000 rewrite kar sakta hai.
**Static Relocation ki Problems**
  1. Protection provide nahi karti: Process illegal addresses generate karke doosre process ya OS ki memory access kar sakta hai.
  2. Relocation difficult hota hai: Ek baar executable ke addresses rewrite ho jaane ke baad process ko doosri physical location par move karna difficult hota hai.
### 6.7 Dynamic Relocation
Dynamic relocation mein address translation runtime par hardware karta hai.
OS process ke liye base register set karta hai. Hardware har memory reference par virtual address mein base add karta hai aur bounds check karta hai.
**Iske benefits:**
  - Runtime par translation hoti hai.
  - Process ko physical memory mein different location par load kiya ja sakta hai.
  - Bounds checking ke through protection milti hai.
  - Process ko stop karke memory copy aur base update karke relocate kiya ja sakta hai.
### 6.8 Memory Management Unit (MMU)
Memory Management Unit (MMU) processor ka hardware part hai jo address translation mein help karta hai.
**Base-and-Bounds model mein MMU:**
  - Base aur bounds registers use karta hai.
  - Virtual address ko physical address mein translate karta hai.
  - Bounds check karta hai.
  - Invalid access par exception mechanism ke saath kaam karta hai.
> More sophisticated memory management techniques mein MMU ke andar additional circuitry add hoti hai.



7. Section 15.4 — Hardware Support: A Summary
Base-and-Bounds ko implement karne ke liye CPU ko kuch specific hardware features provide karne hote hain.
7.1 Two CPU Modes
CPU ko do modes support karne hote hain:
Kernel mode / Privileged mode: OS is mode mein run karta hai aur machine ke privileged operations execute kar sakta hai.
User mode: Normal applications is mode mein run karti hain aur unke operations restricted hote hain.
Processor status word mein ek bit current mode identify kar sakta hai. System call, exception ya interrupt jaise events par CPU mode switch kar sakta hai.
7.2 Base/Bounds Registers
Hardware ko base aur bounds registers provide karne hote hain. OSTEP ke model mein har CPU par ek pair hota hai.
Ye registers:
- Address translation support karte hain.
- Bounds checking support karte hain.
- Different processes ke liye different values rakhte hain.
7.3 Address Translation Circuitry
CPU mein aisi circuitry honi chahiye jo:
1. Virtual address receive kare.
2. Address ko bounds ke against check kare.
3. Valid address ke liye base add kare.
4. Physical address memory system ko provide kare.
Ye hardware har memory reference ke liye translation quickly perform karta hai.
7.4 Privileged Instructions to Update Registers
OS ko process change hone par base aur bounds registers update karne hote hain.
CPU is purpose ke liye special privileged instructions provide karta hai. Ye instructions sirf kernel mode mein execute ho sakti hain.
Agar user-mode process in registers ko change kar sake, to woh apne address translation ko manipulate karke doosri memory locations access karne ki koshish kar sakta hai.
7.5 Privileged Instructions to Register Exception Handlers
OS ko CPU ko batana hota hai ki exception aane par kis handler ko run karna hai.
Iske liye CPU privileged instructions provide karta hai. User-mode applications ko in instructions ka direct access nahi diya jata.
7.6 Ability to Raise Exceptions
CPU ko exception raise karne ki ability honi chahiye jab:
- Process out-of-bounds memory access kare.
- User-mode process privileged instruction execute karne ki koshish kare.
Exception ke baad CPU user program ko stop karke OS handler ko control deta hai.
7.7 Figure 15.3: Hardware Requirements
Hardware requirement	Purpose
Privileged mode	User-mode process ko privileged operations se rokna
Base/bounds registers	Address translation aur bounds checking
Translation circuitry	Virtual address translate karna aur bounds check karna
Privileged register-update instructions	OS ko base/bounds set karne dena
Privileged handler-registration instructions	OS ko exception handlers ke addresses set karne dena
Exception-generation ability	Illegal access aur privileged instruction attempts detect karna
7.8 Free List
Free list ek simple OS data structure hai jo physical memory ke un ranges ki list maintain karti hai jo currently use mein nahi hain.
OS free list ka use:
- Naye processes ko memory allocate karne ke liye.
- Free physical memory track karne ke liye.
- Terminated processes ki memory wapas available karne ke liye.
Example:
Physical range	Status
0–16 KB	OS
16–32 KB	Free
32–48 KB	Process A
48–64 KB	Free
Is example mein free list mein do ranges hongi: 16–32 KB aur 48–64 KB.

8. Section 15.5 — Operating System Issues
Base-and-Bounds model mein hardware translation karta hai, lekin OS ko kuch critical events par involve hona padta hai.
OS ki main responsibilities hain:
1. Process creation par memory allocate karna.
2. Process termination par memory reclaim karna.
3. Context switch par base/bounds save aur restore karna.
4. Process relocation manage karna.
5. Exception handlers provide karna.
6. Boot time par necessary system structures initialize karna.
8.1 Process Creation par Memory Allocation
Jab naya process create hota hai, OS ko uske address space ke liye physical memory mein suitable space find karni hoti hai.
OSTEP ke initial assumptions ke karan physical memory ko equal-sized slots mein divide karke manage kiya ja sakta hai.
OS:
1. Free list mein available space search karta hai.
2. Process ke address space ke liye suitable slot choose karta hai.
3. Slot ko occupied mark karta hai.
4. Process ke liye required memory-management information set karta hai.
Variable-sized address spaces ke case mein allocation more complicated hota hai.
8.2 Process Termination par Memory Reclamation
Process terminate hone par OS uski memory reclaim karta hai.
Termination do tarah se ho sakta hai:
- Process normally exit kare.
- Process misbehavior ke karan forcefully terminate kiya jaye.
Termination ke baad OS:
1. Process ki allocated memory free karta hai.
2. Memory range ko free list mein wapas add karta hai.
3. Process ke associated data structures clean up karta hai.
Isse memory future processes ya OS ke use ke liye available hoti hai.
8.3 Context Switch par Base/Bounds Management
Ek CPU par ek hi base/bounds register pair hota hai. Lekin har process ka base aur bounds different ho sakta hai, kyunki processes physical memory mein alag-alag locations par placed ho sakte hain.
Isliye context switch par OS ko current process ki register values save aur next process ki values restore karni padti hain.
Save
Jab OS Process A ko stop karta hai:
- Base register ki value save karta hai.
- Bounds register ki value save karta hai.
Ye values Process A ke process structure ya Process Control Block (PCB) mein store hoti hain.
Restore
Jab OS Process B ko run karta hai:
- Process B ke PCB se base value read karta hai.
- Process B ke PCB se bounds value read karta hai.
- Dono values CPU registers mein set karta hai.
First time process run hone par bhi OS ko correct base/bounds values set karni hoti hain.
8.4 Process Relocation while Stopped
Jab process run nahi kar raha hota, OS uski address space ko physical memory mein doosri location par move kar sakta hai.
Iske steps:
1. OS process ko deschedule karta hai, yaani uski execution stop karta hai.
2. OS process ki complete address space ko old physical location se new physical location par copy karta hai.
3. OS process structure mein saved base register ko new physical starting address se update karta hai.
4. Process resume hone par OS updated base value CPU mein restore karta hai.
Process ke virtual addresses same rehte hain. Isliye process ko pata nahi chalta ki uski memory physical RAM mein relocate ho chuki hai.
8.5 Exception Handling
OS ko exceptions handle karne ke liye functions ya handlers provide karne hote hain.
OS boot time par privileged instructions ke through exception handlers install karta hai.
Example:
1. Process out-of-bounds memory access karta hai.
2. CPU exception raise karta hai.
3. CPU OS ke out-of-bounds handler ko control deta hai.
4. OS handler situation ko handle karta hai.
5. Is example mein OS likely offending process terminate karega.
OS ko machine protect karni hoti hai, isliye woh illegal memory access ya privileged operations ko allow nahi karta.

9. Boot Time — Figure 15.5
Machine boot hone par abhi koi user program run nahi kar raha hota. OS kernel mode mein system ko ready karne ke liye initial setup karta hai.
9.1 Trap Table Initialize Karna
OS trap table initialize karta hai aur important handlers ke addresses remember karta hai:
- System call handler.
- Timer handler.
- Illegal memory access handler.
- Illegal instruction handler.
Isse CPU ko pata hota hai ki different events ke waqt kis OS handler ko control dena hai.
9.2 Timer Start Karna
OS interrupt timer start karta hai.
Timer ko is tarah set kiya ja sakta hai ki kuch time baad interrupt generate ho. Timer interrupt OS ko CPU control wapas lene aur scheduling karne ka opportunity deta hai.
9.3 Process Table Initialize Karna
OS process table initialize karta hai. Is table mein processes ki management-related information maintain hoti hai.
9.4 Free List Initialize Karna
OS physical memory ke free ranges track karne ke liye free list initialize karta hai.
Is list ka use baad mein process creation aur memory allocation ke waqt hota hai.






10. Runtime — Figure 15.6
Figure 15.6 show karta hai ki process start hone se lekar timer interrupt, context switch aur illegal memory access tak OS aur hardware ka interaction kaise hota hai.
10.1 Process A ko Start Karna
OS Process A ko start karne ke liye ye steps perform karta hai:
1. Process table mein A ke liye entry allocate karta hai.
2. A ke address space ke liye physical memory allocate karta hai.
3. A ke liye base aur bounds registers set karta hai.
4. Process A ke saved CPU registers restore karta hai.
5. CPU ko user mode mein switch karta hai.
6. Process A ke initial Program Counter par jump karta hai.
Ab Process A execute hona start karta hai.
10.2 Process A ka Normal Execution
Process A jab instructions execute karta hai, hardware memory references ko handle karta hai.
Instruction fetch ke waqt:
1. CPU instruction ka virtual address generate karta hai.
2. Hardware address ko translate karta hai.
3. CPU translated physical address se instruction fetch karta hai.
Load/store ke waqt:
1. Process virtual address generate karta hai.
2. Hardware check karta hai ki address bounds ke andar hai ya nahi.
3. Valid hone par hardware base add karke physical address generate karta hai.
4. Memory system us physical address par load/store perform karta hai.
Normal execution ke dauran OS ko har translation ke liye intervene nahi karna padta.
10.3 Timer Interrupt aur Context Switch
Kuch time baad timer interrupt aata hai.
CPU:
1. User mode se kernel mode mein switch karta hai.
2. Timer handler ko control deta hai.
OS:
1. Decide karta hai ki Process A ko stop karke Process B ko run karna hai.
2. Context switch routine call karta hai.
3. Process A ke registers, including base/bounds, uske process structure mein save karta hai.
4. Process B ke registers, including base/bounds, uske process structure se restore karta hai.
5. Return-from-trap ke through Process B ko user mode mein resume karta hai.
6. Process B ke Program Counter par execution continue karata hai.
10.4 Process B ka Illegal Load
Process B ek aisa load operation execute karta hai jiska virtual address bounds ke bahar hai.
Hardware:
1. Address ko check karta hai.
2. Address invalid hone par exception raise karta hai.
3. Process B ki execution stop karta hai.
4. Kernel mode mein switch karke OS trap handler ko control deta hai.
10.5 Process B ko Terminate Karna
OS trap handler illegal memory access ko handle karta hai.
Is example mein OS Process B ko terminate karne ka decision leta hai.
Uske baad OS:
- Process B ki memory deallocate karta hai.
- Process table se B ki entry free karta hai.
Reclaimed memory aur process-table resources future use ke liye available ho sakte hain.
10.6 Figure 15.6 ka Main Lesson
Normal execution mein process directly CPU par run karta hai aur hardware address translation perform karta hai.
OS important events par control leta hai, jaise process start, timer interrupt, context switch aur exception.
Ye limited direct execution ka memory virtualization ke saath use hai: normal execution efficient rehta hai aur OS critical moments par machine ka control maintain karta hai.

11. Base-and-Bounds ki Efficiency aur Limitation
11.1 Efficiency
Base-and-Bounds relatively simple hardware support se kaam kar sakta hai.
Hardware ko:
- Virtual address bounds ke against check karna hota hai.
- Valid hone par base add karna hota hai.
Isliye address translation efficiently perform ho sakti hai.
11.2 Protection
OS aur hardware milkar ensure karte hain ki process apni allowed address space ke bahar memory references generate karke doosre process ya OS ki memory access na kare.
Protection OS ke important goals mein se ek hai. Agar processes freely memory overwrite kar sakein, to woh trap table jaise important OS structures ko damage kar sakte hain.
11.3 Internal Fragmentation
Base-and-Bounds ki important limitation internal fragmentation hai.
Internal fragmentation tab hoti hai jab allocated memory block ke andar kuch memory unused reh jaati hai.
OSTEP ke example mein process ko 32 KB se 48 KB tak 16 KB ka slot diya gaya hai. Lekin process ka stack aur heap bahut bade nahi hain, aur unke beech ka space use nahi ho raha.
Phir bhi poora fixed-size slot process ko allocated hai.
Isliye allocated block ke andar unused memory waste hoti hai.
11.4 Segmentation ki Zaroorat
Base-and-Bounds mein process ko fixed-size continuous physical memory block dena padta hai. Isse unused space bhi allocate ho sakti hai aur internal fragmentation arise hoti hai.
OSTEP ka next step Segmentation hai, jo Base-and-Bounds ka generalization hai. Iska objective process ke different memory regions ko zyada flexible tarike se manage karna aur memory utilization improve karna hai.

12. Complete Chapter Revision — Important Definitions
Term	Definition
Memory virtualization	Physical memory ko aise abstraction ke roop mein provide karna jahan process ko apna private address space dikhe
Virtual address	Process ke perspective se memory location ka address
Physical address	Actual physical memory mein location ka address
Address translation	Virtual address ko physical address mein convert karna
Limited Direct Execution	Program ko mostly directly run karna aur important events par OS ko control dena
Interposition	Interface ke beech mechanism insert karke additional functionality provide karna
Static relocation	Loader ke through executable ke addresses ko run hone se pehle rewrite karna
Dynamic relocation	Runtime par hardware ke through addresses translate karna
Base register	Process ke physical memory starting address ko store karna
Bounds register	Process ke allowed address space ka size store karna
MMU	Address translation mein help karne wala processor hardware
Free list	Physical memory ke free ranges track karne wali OS data structure
PCB	Per-process structure jisme process-related state, including saved register values, store ho sakti hai
Context switch	CPU ka ek process se doosre process par switch karna
Exception	Exceptional condition par CPU ka normal execution se OS handler ko control dena
Internal fragmentation	Allocated memory block ke andar unused memory ka waste hona
Trap table	OS handlers ke addresses se related table, jise events ke waqt control transfer ke liye use kiya jata hai
13. Important Processes — Ek Saath
Process creation
1. OS process table mein entry banata hai.
2. Free list se suitable physical memory range find karta hai.
3. Memory allocate karke occupied mark karta hai.
4. Process ke base/bounds set karta hai.
5. Process ko user mode mein run karata hai.

Context switch
1. Timer interrupt ya doosre event par CPU kernel mode mein aata hai.
2. OS current process ke registers, including base/bounds, save karta hai.
3. OS next process ke saved registers restore karta hai.
4. CPU user mode mein switch karke next process resume karta hai.

Illegal memory access
1. Process out-of-bounds virtual address generate karta hai.
2. Hardware bounds check fail karta hai.
3. CPU exception raise karke OS handler ko control deta hai.
4. OS offending process ko terminate kar sakta hai.
5. OS process ki memory reclaim karke free list mein return karta hai.
