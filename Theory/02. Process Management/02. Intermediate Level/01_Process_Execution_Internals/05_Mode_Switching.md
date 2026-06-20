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
1. User process CPU par execute ho raha hota hai.
2. Koi trigger aata hai:
   - System call
   - Hardware interrupt
   - Exception/Fault
3. CPU current instruction ko complete karta hai (zyadatar cases me).
4. CPU detect karta hai ki normal execution continue nahi karna hai.
5. CPU apne privilege checking logic ko activate karta hai.
6. CPU user mode bit/ring level ko read karta hai.
7. CPU hardware-defined transition sequence start karta hai.
8. CPU user mode ka current execution point (Program Counter/Instruction Pointer) save karta hai.
9. CPU current processor status/flags save karta hai.
10. CPU required control registers ka state preserve karta hai (architecture dependent).
11. CPU kernel stack identify karta hai.
12. CPU user stack se kernel stack par switch karta hai.
13. CPU privilege level change karta hai (Ring 3 → Ring 0 in x86).
14. CPU interrupt/exception/system call vector number identify karta hai.
15. CPU vector table lookup karta hai.
16. CPU corresponding handler address nikalta hai.
17. CPU Program Counter ko handler address se load karta hai.
18. CPU ab kernel code execute karna start karta hai.
19. Kernel ka entry stub additional registers save karta hai.
20. Kernel complete CPU context preserve karta hai agar zarurat ho.
21. Kernel reason identify karta hai (interrupt, syscall, exception).
22. Kernel appropriate handler execute karta hai.
23. Kernel requested work complete karta hai.
24. Zarurat padne par scheduler invoke ho sakta hai.
25. Agar same process continue karega to uska context ready rakha jata hai.
26. Agar dusra process run hoga to context switch perform hota hai.
**Kernel Mode → User Mode Return**
27. Kernel apna kaam complete karta hai.
28. Kernel return path prepare karta hai.
29. Kernel pending signals/events check karta hai.
30. Kernel restore karne wale registers identify karta hai.
31. Saved CPU registers restore kiye jate hain.
32. Saved processor flags restore kiye jate hain.
33. Saved instruction pointer restore kiya jata hai.
34. CPU kernel stack se user stack par wapas switch karta hai.
35. CPU privilege level restore karta hai (Ring 0 → Ring 3).
36. CPU special return instruction execute karta hai (iret, sysret, etc. architecture dependent).
37. CPU hardware validation karta hai ki return legal hai.
38. CPU user mode bit set karta hai.
39. CPU user process ka next instruction fetch karta hai.
40. User process execution continue kar deta hai.

