# 1. Interface
Ya ek defined boundary/method set hota hai jiske through OS aur applications I/O devices sa interact karte hai bina hardware ki internal complexity jaane.  
## 1.1 Interface Layers 
1. User-level Interface (Application Interface) - Ya woh top layer hoti hai jahan applications (user programs) OS sa I/O services ko standard APIs ya system calls ka through request karti hai, bina hardware details jane.
   - **Role**
     1. Applications aur OS ka beech standard communication method provide karta hai.
     2. Programmers ko hardware ki complexity sa completely abstract karta hai.
     3. I/O operations ko simple functions (read/write/open) ka form ma available karata hai.
     4. User space aur kernel space ka beech controlled transition ensure karta hai.
     5. System resources ko safe aur restricted access ka through manage karne me help karta hai. 

  - **Internal Mechanism**
    1. Application user-level function call karta hai (jaise read(), printf(), fopen()).
    2. Agar API/library function hai to wo internally system call ko invoke karta hai.
    3. System call ke liye CPU ek trap/interrupt instruction execute karta hai (user space → kernel space transition).
    4. Control transfer hota hai kernel ke System Call Interface (system call handler) ko.
    5. Kernel system call number ko decode karke required service identify karta hai.
    6. Kernel request ko validate karta hai (permissions, arguments, safety checks).
    7. Kernel I/O subsystem request ko appropriate device/file/network handler tak route karta hai.
    8. Agar I/O request hai to device driver ko call kiya jata hai.
    9. Device driver hardware/controller se interaction karke operation perform karwata hai.
    10. Operation complete hone par result kernel ko return hota hai.
    11. Kernel result ko process karke user space format me convert karta hai.
    12. Control wapas system call return ke through application ko mil jata hai.

2. Kernel Level Interface (OS internal Interface) - Kernel-Level Interface ek OS ka internal interface layer hota hai jo system calls se aayi requests ko process karke unhe device drivers, file system, aur I/O subsystem tak route karta hai, aur system resources ko manage karta hai.
  - **Role**
  1. User level system calls ko receive karke process karta hai.
  2. Requests ko validate karta hai (permissions, safety, correctness).
  3. I/O requests ko kernel I/O subsystem tak route karta hai.
  4. Appropriate device driver ko select aur invoke karta hai.
  5. Buffering aur caching ka management karta hai performance improve karne ka lia. 
  - **Internal Mechanism**
    1. User application system call (read/write/open) trigger karti hai.
    2. CPU trap/interrupt instruction ke through user space se kernel space me switch karta hai.
    3. Control kernel ke System Call Interface (handler) ko milta hai.
    4. Kernel system call number ko decode karke request identify karta hai.
    5. Kernel request ke arguments ko validate karta hai (address, size, permissions).
    6. Kernel check karta hai ki request file, device ya memory se related hai.
    7. Request ko kernel I/O subsystem ya relevant kernel module ko forward kiya jata hai.
    8. Kernel I/O subsystem request ko queue, schedule aur optimize karta hai.
    9. Appropriate device driver select karke call kiya jata hai.
    10. Device driver hardware/controller ke saath communication karta hai.
    11. Operation complete hone par driver result kernel ko return karta hai.
    12. Kernel result ko process karke user-space format me convert karta hai.
    13. CPU system call return ke through control wapas application ko de deta hai.
        
3. Hardware-level Interface (Device Interface) - Hardware-Level Interface woh lowest-level interface hota hai jo OS/device drivers aur actual hardware (CPU, memory, I/O devices) ke beech direct communication define karta hai through registers, buses, and control signals.
  - **Role** 
    1. OS aur actual hardware devices ke beech direct communication enable karta hai.
    2. Software commands (driver/OS) ko physical signals me convert karta hai.
    3. Data transfer ke liye low-level registers (control, status, data) provide karta hai.
    4. CPU, memory aur I/O devices ke beech bus-based communication manage karta hai.
    5. Interrupt signals generate aur handle karne ka mechanism provide karta hai.
  - **Internal Mechanism**
    1. Device driver OS se hardware operation ka request receive karta hai (read/write/control).
    2. Driver hardware ke control, data aur status registers ko configure karta hai.
    3. Control signals bus (PCIe/USB/SATA/system bus) ke through device controller tak pahunchte hain.
    4. Device controller request receive karke internal hardware operation start karta hai.
    5. Hardware device actual physical operation perform karta hai (data read/write/transfer).
    6. Operation ke dauran device status register update hota rehta hai (busy/ready/done).
    7. Agar DMA use ho raha ho to DMA controller memory aur device ke beech direct data transfer karta hai.
    8. Operation complete hone par device controller status register ko DONE/READY set karta hai.
    9. Device controller CPU ko interrupt signal (IRQ) generate karta hai (agar required ho).
    10. Interrupt CPU tak pahunchta hai aur OS interrupt handler execute hota hai.
    11. Driver final status aur data ko hardware registers se read karta hai.
    12. Result kernel ko return hota hai aur OS user application ko response bhej deta hai.

# 2. Addressing
Ya ek mechanism hai jissa CPU/OS decide karta hai ki kaunsa device, memory location ya register access karna hai. 
## 2.1 Types of Addressing
#### 1. Memory Mapped I/O

#### 2. Port Mapped I/O
