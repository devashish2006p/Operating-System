# 1. Device Communication
Device communication woh process hai jisme CPU/OS aur I/O devices ek dusre ka sath data, commands aur status information exchange karta hai taki requested operation perform ho sake. 

# 2. Methods of Communication

## 2.1 Programmed I/O (Polling)
Ya ek communication method hai jisme CPU khud continuously device ko check karta rehta hai ki device ready hai ya nahi aur jab ready hota hai tab data transfer karta hai. 
### Internal Mechanism
1. CPU pehle I/O instruction execute karta hai (read/write request generate hoti hai).
2. OS/driver device ko Control Register ke through command send karta hai (jaise READ operation start).
3. Device controller command receive karke operation start karta hai aur Status Register ko BUSY set kar deta hai.
4. CPU ek polling loop start karta hai jisme wo continuously Status Register check karta rehta hai.
5. CPU repeatedly device ka status read karta hai jab tak status READY ya DONE na ho jaye.
6. Device apna internal operation complete karta hai (disk read, keyboard input, etc.).
7. Operation complete hone par device controller Status Register ko READY/DONE set karta hai.
8. CPU polling loop se exit karta hai jab READY status detect hota hai.
9. CPU Data Register se actual data read ya write karta hai (data transfer hota hai).
10. Agar output operation hai to CPU Data Register me data write karta hai jise device consume karta hai.
11. Operation complete hone ke baad control wapas OS/application ko return kar diya jata hai.
    
## 2.2 Interrupt-Driven I/O
Ya ek mechanism hai jisme device CPU ko interrupt bhejta hai jab ushko attention cahiye hota hai, isiliye CPU ko continuously polling nhi karni parti. 
### Internal Mechanism 
1. CPU pehle I/O request initiate karta hai aur device ko command send karta hai (via control register/driver).
2. Device controller request receive karke operation start karta hai aur CPU ko free kar deta hai (CPU wait nahi karta).
3. CPU apna normal execution (process scheduling/other instructions) continue karta rehta hai.
4. Device apna I/O operation independently complete karta hai (read/write/transfer).
5. Operation complete hone par device controller CPU ko interrupt signal generate karta hai.
6. CPU ko current execution se temporarily stop karke interrupt service routine (ISR) execute karni padti hai.
7. ISR ke andar device driver interrupt ko handle karta hai aur status check karta hai.
8. Driver Data Register se data read/write karta hai ya result collect karta hai.
9. Kernel I/O subsystem result ko process karke OS ko update karta hai.
10. Interrupt handle hone ke baad CPU wapas apne previous execution par return kar jata hai.

## 2.3 DMA (Direct Memory Access) 
Ya ek I/O communication mechanism hai jisme data transfer directly device aur main memory (RAM) ka beech hota hai, CPU sirf transfer ko initiate karta hai aur baad ma control de deta hai. 
### Internal Mechanism
1. CPU/OS driver DMA controller ko I/O request setup karta hai (source address, destination address, data size aur direction set karke).
2. DMA controller request receive karke system bus ke liye ready ho jata hai aur transfer parameters store kar leta hai.
3. CPU DMA ko permission deta hai ki wo memory bus control (bus mastering) le sakta hai jab transfer start hoga.
4. DMA controller device ko signal deta hai aur I/O device ready state me aa jata hai data transfer ke liye.
5. DMA controller memory aur device ke beech direct data transfer start karta hai bina CPU involvement ke.
6. Transfer ke dauran DMA controller memory bus ko control karta hai aur data blocks ko sequentially move karta hai.
7. CPU is dauran completely free hota hai aur apne normal instructions execute karta rehta hai (parallel execution).
8. DMA controller transfer progress track karta hai (current address pointer aur remaining byte count maintain karke).
9. Jab saara data transfer complete ho jata hai, DMA controller apna status update karta hai (DONE flag set karta hai).
10. DMA controller CPU ko interrupt bhejta hai taaki completion notify ho sake.
11. CPU interrupt handle karta hai aur OS/driver final status check karta hai.
12. OS I/O operation ko complete mark karta hai aur application ko result return kar deta hai.
