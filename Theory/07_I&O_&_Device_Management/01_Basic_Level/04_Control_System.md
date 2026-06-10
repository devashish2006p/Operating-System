# 1. I/O Control System 
I/O Control System ek mechanism hota hai jo OS, device controllers aur I/O devices ke beech communication, commands, status monitoring aur data transfer operations ko control aur coordinate karta hai taaki I/O operations sahi tarike se perform ho saken.

# 2. What does it controlls?
1. I/O Commands - Device ko kya kaam karna hai (read, write, start, stop).
2. Device Status - Device busy hai, ready hai, done hai ya error me hai.
3. Data Transfer Operations - Data device aur memory/CPU ka beech kaisa transfer hoga.
4. Device Access Control - Kaunsa process/device ko access kar shakta hai.
5. Synchronization - CPU aur device ka beech timing/co-ordination maintain karna.
6. Interrupt Handling - Device sa aane wale interrupts ko manage karna.
7. DMA co-ordnation - DMA transfers ko setup aur monitor karna.
8. Error Handling - I/O operation fail hone par errors detect aur handle karna.
9. Request scheduling/queuing - Multiple I/O requests ko manage karna.
10. Device communication flow - OS, driver, controller aur device ka beech communication co-ordinate karna. 

# 3. Internal Mechanism 
1. Application kisi I/O operation (read/write/send/receive) ki request generate karti hai.
2. Application API ya library function ke through system call invoke karti hai.
3. CPU trap/system call instruction execute karke user mode se kernel mode me switch karta hai.
4. System Call Interface request ko receive karta hai aur kernel ko transfer karta hai.
5. Kernel I/O Subsystem request ko validate karta hai (permissions, parameters, safety checks).
6. Kernel identify karta hai ki request kis device ya resource ke liye hai.
7. Kernel appropriate device driver ko select karke request forward karta hai.
8. Device driver generic OS request ko device-specific commands me convert karta hai.
9. Driver device controller ke control registers ko configure karta hai.
10. Device controller command receive karke hardware operation start karta hai.
11. Agar Programmed I/O use ho raha hai to CPU status register poll karta rehta hai.
12. Agar Interrupt-Driven I/O use ho raha hai to CPU apna dusra kaam continue karta hai aur device completion par interrupt bhejta hai.
13. Agar DMA use ho raha hai to DMA controller memory aur device ke beech direct transfer perform karta hai.
14. Operation ke dauran device controller status registers update karta rehta hai (busy, ready, error, done).
15. Device actual hardware operation perform karta hai (disk read, keyboard input, network receive, printer output, etc.).
16. Data transfer CPU, Device Controller ya DMA ke through complete hota hai.
17. Operation complete hone par device controller DONE status set karta hai.
18. Zarurat padne par device controller interrupt generate karta hai.
19. Interrupt aane par CPU Interrupt Service Routine (ISR) execute karta hai.
20. Device driver final status aur result device controller ke registers se read karta hai.
21. Driver result ko OS-understandable format me convert karta hai.
22. Kernel I/O Subsystem result ko process karta hai aur request ko complete mark karta hai.
23. System Call Interface result ko user space me return karne ki preparation karta hai.
24. CPU kernel mode se user mode me return karta hai.
25. Application ko final result, data ya error status mil jata hai.
