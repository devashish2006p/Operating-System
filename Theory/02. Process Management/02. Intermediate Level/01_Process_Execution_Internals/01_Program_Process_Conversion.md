# 1. Program Process Conversion
Program process conversion means program ko OS dwara memory me load karke execution ke liye active process banana.

# 2. Internal Mechanism 
1. User Execute Request deta hai.
  - Keyboard controller key press ko detect karta hai aur uska scan code generate karta hai.
  - Keyboard scan code ko USB packet ka roop ma USB host controller ko bhejta hai.
  - USB host controller key data receive karta hai aur CPU ko interrupt request (IRQ) bhejta hai.
  - CPU interrupt receive karta hai aur current instruction complete karke interrupt service routine (ISR) execute karna shuru karta hai.
  - USB keyboard driver USB controller ke status aur data registers ko read karta hai.
  - USB keyboard driver received key data ko kernel ke input subsystem ko forward karta hai.
  - Kernel input subsystem scan code ko process karta hai aur key event generate karta hai.
  - TTY subsystem key event ko process karke TTY buffer me store karta hai.
  - User jab Enter press karta hai to TTY subsystem poori input line ko ready mark karta hai.
  - Terminal application kernel buffer se input line read karti hai.
  - Terminal application input line ko shell process tak pahunchati hai.
  - Shell command ko parse karti hai aur identify karti hai ki executable program run karna hai.
  - Shell executable ka path, arguments aur environment prepare karti hai.
  - Shell execve() routine ko call karti hai.
  - CPU shell ke instructions execute karte hue syscall instruction tak pahunchta hai.
  - syscall instruction execute hote hi system call trigger ho jata hai aur CPU User Mode se Kernel Mode me switch kar jata hai.

2. System call Trigger hota hai. Shell kudh program ko directly RAM ma nahi dal shakta, shell kernel ko request karta hai. (Linux ma commonly fork(), execve() use hota hai), jishke baad user mode -> kernel mode switch hota hai.
3. Kernel Program file locate karta hai through file system. File system check karta hai agr file mil gya to file system file kernel ko de deta hai. 
4. Executable Format Check hota hai. Kernel file ko open karke header check karta hai. (Linux ma usually ELF aur windows ma PE format use hota hai). Kernel verify karta hai hai "valid executable hai ya nahi", "architecture match, "corrupt to nhi hai". Agr fail to error otherwise continue.
5. Process Control Block (PCB) create hota hai. Ab os decide karta hai "Program ko process bnana hai" Sbse pehle major object : PCB create karta hai, fir PCB ma PID, PPID, State, Priority, Registers, memory info, scheduling info, open files rakhe jate hai. Ab process officially exist karna start kar rha hai.
6. PID assign hota hai process ko.
7. Kernel process ka lia virtual address space create karta hai.
8. Page tables create hoti hai.
9. Program code memory ma load hota hai.
10. Stack create hota hai. Jisme function calls, local variables, return addresses rekhe jate hai.
11. Heap Create hota hai.
12. Kernel stack create hota hai. Har process ka lia ek special stack hota hai, joki sirf kernel mode operations ka lia hota hai.
13. CPU context initialize hota hai.
14. Process ka executable header ma entry point set hota hai, joki cpu ko btata hai execution kaha sa start karna hai.
15. Ab process state New hota hai.
16. Scheduler process ko ready queue ma insert karta hai. Ab process RAM ma hai. Resources allocated hote hai aur CPU ka wait kar rha hota hai process.
17. Scheduler Select karta hai process ko.
18. Context switch hota hai aur current process save hota hai aur naye process ka context restore hota hai.
19. Ab process ka state Running ho jata hai.
20. First instruction execute hoti hai 
