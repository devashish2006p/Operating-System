# 1. I/O System 
I/O System OS aur Computer Architecture ka woh part hota hai jo CPU, memory aur external devices (jaisa keyboard, mouse, disk, printer) ka beech data transfer aur communication ko controllers, interrupts aur DMA ka through efficiently aur safely manage karta hai. 

# 2. Types of I/O Devices 
## 2.1 Block Devices 
Block device woh I/O device hota hai jo data ko fixed-size blocks (chunks) me read/write karta hai aur jisme random access possible hota hai.

## 2.2 Character Devices 
Character device woh I/O device hota hai jo data ko continuous stream (byte-by-byte) form me read/write karta hai, bina fixed-size blocks ke.

## 2.3 Network Devices 
Network device woh I/O device hota hai jo computer ko network ke through dusre computers ya systems ke saath data packets ke form me communicate karne deta hai, jahan data OS network stack ke through send/receive hota hai.

## 2.4 Virtual/Pseudo Devices 
Virtual (ya pseudo) devices woh I/O devices hote hain jo real hardware nahi hote, balki OS kernel software ke through simulate karta hai taaki applications ko standard device-like interface mil sake.

# 3. Device Abstraction
Device abstraction ka matlab hai same simple OS interface (jaise read/write) ko use karke different devices ko access karna, jahan OS har device ke liye internally alag implementation handle karta hai.
# 4. Device Controller 

# 5. Memory Vs Device Concept 
