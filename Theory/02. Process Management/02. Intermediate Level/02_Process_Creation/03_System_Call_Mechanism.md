# 1. System Call 
System Call ek controlled interface (request mechanism) hai jiske through user-space program Kernel se kisi bhi Kernel service ko perform karne ki request karta hai.

System Call ki need kyu hoti hai.
User Mode vs Kernel Mode (system call ke context me).
System Call Wrapper.
System Call ABI (arguments passing convention).
System Call Number.
syscall instruction.
CPU ka privilege switch (Ring 3 → Ring 0).
Kernel Entry Point.
Register Saving.
Kernel Stack.
System Call Table.
System Call Dispatcher.
System Call Handler.
Argument Validation.
User Memory Validation (copy_from_user() / copy_to_user()).
Return Value Mechanism.
Register Restore.
sysret (ya architecture-specific return instruction).
User Mode me return.

























# System Call Internal Mechanism
1. User program ko kernel service ki zarurat padti hai.
2. User program system call wrapper (libc ya direct syscall wrapper) ko call karta hai.
3. Wrapper system call number identify karta hai.
4. Wrapper system call ke arguments CPU registers me load karta hai.
5. Wrapper syscall instruction execute karta hai.
6. CPU syscall instruction ko detect karta hai.
7. CPU privilege level Ring 3 se Ring 0 me change karta hai.
8. CPU predefined kernel entry address par control transfer karta hai.
9. Kernel ka system call entry code execute hona shuru hota hai.
10. Kernel user-space execution state ko safe rakhne ke liye required CPU registers save karta hai.
11. Kernel kernel stack ka use karke apna execution environment establish karta hai.
12. Kernel RAX register se system call number read karta hai.
13. Kernel check karta hai ki system call number valid hai ya nahi.
14. Kernel syscall table me us number ki entry lookup karta hai.
15. Syscall table se corresponding kernel handler function ka address milta hai.
16. Kernel dispatcher us handler function ko call karta hai.
17. Handler registers se arguments read karta hai.
18. Handler arguments ko validate karta hai.
19. Handler user-space pointers (agar diye gaye hain) ko verify karta hai.
20. Zarurat padne par copy_from_user() ya copy_to_user() ke through user aur kernel memory ke beech safely data transfer karta hai.
21. Handler actual kernel operation perform karta hai.
22. Operation complete hone ke baad success ya error result prepare kiya jata hai.
23. Return value RAX register me store ki jati hai.
24. Kernel entry code saved registers restore karta hai.
25. Kernel sysret (ya architecture-specific return instruction) execute karta hai.
26. CPU privilege level Ring 0 se Ring 3 me wapas switch karta hai.
27. CPU control ko user program ki next instruction par return kar deta hai.
