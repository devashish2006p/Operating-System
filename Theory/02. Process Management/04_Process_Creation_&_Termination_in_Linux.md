# Part 1 : Process Creation sa Pehle kya hota hai?
1. Jab user C program likhta hai to user source code create karta hai jo initially ek odinary file hota hai.
2. Compiler source code ko machine instructions mein translate karta hai aur executable file generate karta hai.
3. Executable file disk par stored hoti hai; is stage par ushka apna koi runing process nahi hota.
4. Jab operating system us executable ko execute karne ka liye load karta hai tab ushke execution ka liya process enviroment establish hota hai.
5. Process sirf executable instructions ka naam nahi hai, ishmein address space, registers, execution state, kernel managed resources, security credentials aur scheduling information bhi hota hai.
6. Jab linux boot hota hai, kernel initialization ka baad userspace enviroment start hota hai.
7. Login ka baad user shell, jaisa Bash ek already running process hota hai.
8. Shell apne process ka ander command input receive karta hai.
9. Jab tum ls-l type karte ho tab shell command ko parse karta hai aur identify karta hai ki kaunsa executable run karna hai.
10. Shell generally khud ko terminate nahi karta qoki command complete hone ka baad bhi ushko next command accept karni hoti hai.
11. Isiliye normal external command execute karne ka lia shell ek child process create karta hai.

# Part 2 : Process creation 
1. Bash apne userspace execution context mein fork() call karta hai.
2. Linux userspace mein fork() generally C library (glibc) ka wrapper hota hai.
3. Wrapper kernel sa process creation request karne ka liye system call mechanism use karta hai.
4. CPU userspace sa kernel mode mein transition karta hai.
5. Kernel system call entry mechanism ka through control receive karta hai.
6. Kernel system call arguments aur calling context process karta hai.
7. Linux implementation mein modern glibc ka fork() wrapper internally clone() system call mechanism use karta hai, appropriate flags ka sath.
8. Kernel process creation ka liye internel process management routines execute karta hai.
9. Modern Linux mein is process ka important internel function copy_process() hai.
10. Kernel naye task ka liye kernel managed task structure allocate karta hai.
11. Linux mein is primary task structure ka naam task_struct hai.
12. task_struct process ki kernel side representation maintain karta hai.
13. Ishmein PID/TGID, process state, scheduling information, credentials, relationships, resource pointers aur execution related information hoti hai.
14. Kernel Child task ka liye required kernel stack aur architecture specific execution context prepare karta hai.
15. Parent aur child ka task structures alag hota hai, qoki dono independant execution entities hai.
16. Kernel naye task ke liye PID allocate karta hai.
17. PID Linux ke PID management mechanism ke through manage hota hai.
18. PID namespace ke context mein PID ki identity determine hoti hai.
19. Child ka PID parent ke PID se different hota hai.
20. Kernel parent-child relationship maintain karta hai.
21. Child ke task_struct mein parent-related references aur process identification information maintain hoti hai.
22. Kernel PID ko userspace mein visible process identifier ke roop mein expose karta hai.
23. Kernel child ke liye process-related resource references prepare karta hai.
24. Parent aur child ke file descriptor tables ko normally logically separate copies milti hain.
25. Lekin inherited file descriptors ke corresponding open file descriptions generally shared hoti hain.
26. Iska matlab parent aur child ke descriptors separate ho sakte hain, lekin woh same underlying open file description ko refer kar sakte hain.
27. Filesystem context, current working directory aur root directory jaise resources fork ke normal behavior mein inherit hote hain, generally references share karke.
28. Signal-related structures aur memory-management structures bhi fork ke according initialize ya duplicate hote hain.
29. Kernel credentials aur security-related state bhi child ke liye establish karta hai.
30. Child ke scheduling-related fields initialize kiye jaate hain, taaki scheduler usko manage kar sake.

# Part 3 : Memory Management
1. Jab Bash execute ho rha hota hai to ushke pass ek virtual address space hota hai.
2. Ya address space process ke code, data, heap, stack, memory mappings aur other mapped regions ko represent karta hai.
3. Linux kernel is address space ko manage karne ka liye mm_struct data structure use karta hai.
4. mm_struct process ki memory-management information maintain karta hai.
5. Process ka virtual addresses ko physical memory sa map karne ka liye page tables use hota hai.
6. Page tables virtual pages aur physical page frames ka beech maping establish karti hai.
7. Jab fork() execute hota hai, kernel child ka liye seperate memory-management context establish karta hai.
8. Child ko parent ka mm_struct ki logically seperate copy milti hai.
9. Kernel parent aur child ka memory mappings ko establish karta hai.
10. Har physical memory page ki immediate independent copy generally nahi banayi jati hai.
11. Linux is process mein Copy-on-Write (COW) mechanism use karta hai.
12. COW mein parent aur child initially same physical memory pages ko map kar shakta hai.
13. Dono ka page tables seperate hota hai, lekin initially unmein corresponding virtual addresses same physical pages ko point kar shakte hai.
14. In mappings ko appropriately write protected kiya jata hai, taki modification detect ho sake.
15. Abhi tak parent aur child ka pass logically seperate address spaces hain, lein physical memory pages share ho rahe hai.
- **Child jab memory modify karta hai tab kya hota hai?**
1. Child process kisi C variable ya memory location par write operation perform karta hai.
2. CPU virtual address ko translate karte waqt page table entry mein write permission check karta hai.
3. Agar page write-protected hai, toh CPU page fault generate karta hai.
4. CPU control kernel ke page-fault handler ko transfer karta hai.
5. Kernel check karta hai ki fault COW protection ki wajah se hua hai ya kisi aur reason se.
6. Agar valid COW fault hai, toh kernel affected physical page ke liye private copy allocate karta hai.
7. Original page ka content new physical page mein copy kiya jaata hai.
8. Child ki page table update hoti hai, taaki uska virtual address ab new physical page ko refer kare.
9. Parent ki mapping original physical page par hi rehti hai.
10. Kernel appropriate permissions update karta hai aur child ki interrupted instruction ko retry karne deta hai.
- **fork() return hone par kya hota hai?**
1. Kernel child ke execution context ko prepare karta hai.
2. Parent ke execution context mein fork() ka return value child PID hota hai.
3. Child ke execution context mein fork() ka return value 0 hota hai.
4. Kernel dono processes ko execution ke liye eligible bana sakta hai.
5. Dono independent processes hain, jinke paas separate PID aur execution state hai.
6. Kaunsa process pehle CPU par execute hoga, ye scheduler decide karta hai.
7. Isliye child ka printf() parent ke printf() se pehle ya baad mein execute ho sakta hai, unless synchronization impose ki gayi ho.

# Part 4. execve(): Existing process ke andar new program kaise load hota hai?
Ab hum us stage par hain jahan child process create ho chuka hai, lekin abhi bhi uske andar inherited program execute ho raha hai.
- **exec() call ka purpose**
1. Child process decide karta hai ki usko koi different executable program execute karna hai.
2. Child exec() family ka function call karta hai.
3. execl(), execv(), execvp() etc. different interfaces provide karte hain.
4. In sabka underlying kernel-level execution mechanism execve() system call hai, ya related variants.
5. execve() existing calling process ko new program image mein transform karta hai.
6. Ye new PID allocate karke new process create nahi karta.
- **execve() kernel mein enter karta hai**
1. Userspace wrapper arguments aur environment prepare karta hai.
2. System call mechanism CPU ko kernel mode mein transfer karta hai.
3. Kernel executable pathname, arguments aur environment information receive karta hai.
4. Kernel executable file ko access karne ki permission check karta hai.
5. File open aur validate karne ke baad kernel uska executable format identify karta hai.
6. Linux mein executable format handling ke liye Binary Format handlers use hote hain.
7. ELF executable ke case mein Linux ELF loader use karta hai.
- **Purane memory image ka replacement**
1. Kernel successful exec operation ke liye old program image ko replace karne ki preparation karta hai.
2. Existing process ki purani userspace memory mappings remove ya replace ki jaati hain.
3. Purane code, data, heap aur stack mappings new program image ka part nahi rehte.
4. Kernel new executable ke liye required memory mappings establish karta hai.
5. New executable ke code aur data segments memory mein map kiye jaate hain.
6. Writable segments aur other required mappings establish hote hain.
7. Heap aur stack ke liye new process image prepare hota hai.
8. Shared libraries aur dynamic linker ki mappings bhi required hone par establish hoti hain.
> Ek important detail: ELF executable ka har byte necessarily immediately RAM mein copy nahi hota. Linux demand paging aur memory mapping ka use karta hai; required pages execution ke time load ho sakte hain.
- **New program ka initial execution environment**
1. Kernel new program ke arguments ko userspace stack par arrange karta hai.
2. Environment variables bhi new program ke initial stack environment mein arrange kiye jaate hain.
3. Auxiliary vector (auxv) jaise startup information bhi provide ki jaati hai.
4. CPU ka instruction pointer new program ke appropriate entry point par set kiya jaata hai.
5. Process ke execution context ko new program ke startup ke according update kiya jaata hai.
6. Successful execve() purane program ke code par return nahi karta.
7. Kernel userspace mein return karte waqt new program ka execution start karata hai.
8. Dynamically linked ELF program ke case mein initial entry point usually dynamic linker ka hota hai.
9. Dynamic linker required shared libraries resolve aur initialize karta hai.
10. Uske baad C runtime startup mechanism ke through main() execute hota hai.
