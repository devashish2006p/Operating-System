# 1. Process Address Space 
Process Address Space wo virtual memory layout hai jo kernel har process ke liye create karta hai aur jisme code, data, heap, stack aur dusre memory regions organized rehte hain.

# 2. Componenets of Address Space 
1. Text - Text Segment Process Address Space ka wo region hai jahan program ka machine code (instructions) store hota hai. CPU ko jo instructions execute karni hoti hai wo text segment ma sa he instruction uthata hai. 
2. ROData - ROData(Read Only Data) ka mtlb hota hai aisa data jo program ka ander hota hai lekin runtime ma modify nhi kiya jana cahiye. 
3. Data - Yaha initialized global aur initialized static variables store hota hai. Ex - int x = 10; 
4. BSS (Block Started by Symbol) - BSS Segment me Uninitialized Global aur Uninitialized Static Variables store hote hain.
5. Heap - Ya Process Address Space ka wo region hai jahan runtime par dynamically allocated memory rakhi jati hai. Jab program execution ka dauran memory maangta hai to wo Heap sa milti hai. 
6. Memory Mapped Area - Memory Mapped Area process ke address space me ye represent karta hai ki kaun-kaun se extra memory regions is process ke saath map kiye gaye hain.
7. Shared Libraries - Shared Library ek aisi file hoti hai jisme reusable machine code (functions/instructions) store hote hain aur jise ek hi samay par multiple processes share karke use kar sakte hain.
8. Stack - Stack Process Address Space ka wo region hai jahan function calls se related temporary data store hota hai.
9. Kernel Space - Kernel Space virtual address space ka wo privileged region hai jahan Operating System ka kernel code, kernel data, device drivers aur core system components mapped hote hain, aur jise sirf kernel mode me CPU access kar sakta hai.

# 3. Tools for analysis of Process Address Space 
1. /proc/<PID>/maps - /proc/<PID>/maps Linux ke /proc virtual filesystem ki ek pseudo-file hai jo Kernel se real-time me us running process ke memory mappings (address ranges, permissions, mapped files aur regions) ki information read karke user ko dikhati hai; ise Process Address Space ko inspect aur analyze karne ke liye use kiya jata hai.

2. /proc/<PID>/smaps - /proc/<PID>/smaps Linux ke /proc virtual filesystem ki ek pseudo-file hai jo Kernel se real-time me process ke har memory mapping ki detailed memory usage (RSS, PSS, Private/Shared pages, Swap, permissions adi) ki information read karke user ko dikhati hai; ise Process Address Space ka detailed analysis aur memory consumption inspect karne ke liye use kiya jata hai.

3. pmap - pmap ek Linux user-space utility hai jo /proc/<PID>/maps aur related kernel memory information ko read karke running process ke memory layout ko human-readable summary ke roop me dikhati hai; ise Process Address Space ko jaldi inspect aur analyze karne ke liye use kiya jata hai.

4. gdb - gdb (GNU Debugger) ek user-space debugging tool hai jo Kernel ke debugging interfaces (jaise ptrace) ke through running process ko control aur inspect karta hai, uski memory, registers aur execution state ko real-time me analyze karta hai; ise debugging, reverse engineering aur Process Address Space ko inspect karne ke liye use kiya jata hai.
