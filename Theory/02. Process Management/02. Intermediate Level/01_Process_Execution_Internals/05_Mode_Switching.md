# 1. Mode Switch 
Mode Switching woh process hai jisme CPU ek hi process ko execute karte hue apna privilege level (User Mode ↔ Kernel Mode) change karta hai taaki protected system operations safely perform kiye ja saken.
# 2. Types of CPU Modes
There are two types of Modes:- 
1) User Mode - Normal applications yahan execute hota hai, direct hardware access allowed nahi hota aur restricted privilages hote hai. 
2) Kernel Mode - OS (Kernel) yahan execute hota hai, full hardware access hota hai aur memory, CPU, devices sab control kar shakte hai. 
# 3. Reasons of Mode Switch
1. System Call - Jab user program OS se service maangta hai (jaise file read/write), to CPU user mode se kernel mode me switch karta hai.

2. Interrupt - Jab hardware CPU ko signal bhejta hai (jaise timer, keyboard, disk), to CPU current execution rok kar kernel mode me chala jata hai.

3. Exception/Trap - Jab program execution ke dauran koi error ya special condition aati hai (jaise divide by zero ya page fault), to CPU kernel mode me switch karta hai.
   
4. Privileged Instruction Voilation - Jab user mode me koi restricted operation chalane ki koshish hoti hai (jaise direct hardware access), to CPU protection ke liye kernel mode me switch kar deta hai.
   
# 4. Internal Mechanism of Mode Switch

# 5. Mode Switch Overhead
