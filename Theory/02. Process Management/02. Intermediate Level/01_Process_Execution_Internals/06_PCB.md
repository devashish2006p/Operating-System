# 1. PCB Creation Flow
## Phase 1 : Process Creation Request
1. Process creation ki request aati hai.
2. Shell fork() call karta hai.
3. fork() system call invoke karta hai
4. CPU user mode-> kernel mode switch karta hai.
5. Control kernel ka fork() handler ko milta hai.
6. Kernel naya process create karta hai.

## Phase 2 : Kernel PCB create karne ka decision leta hai. 
1. Kernel Verify karta hai ki naya process create karna allowed hai ya nahi.
    - User/process limits : Jab koi process naya process create karna chahta hai, kernel check karta hai ki user ya process apni allowed maximum process limit ko cross to nahi kar raha. Agar limit cross ho gayi ho, to naya process create nahi kiya jayega.
    - Permissions : Kernel check karta hai ki jo process process-creation request bhej raha hai, uske paas required authority aur permissions hain ya nahi. Agar security rules ke hisab se process creation allow nahi hai, to request reject kar di jati hai.
    - Available memory : Kernel check karta hai ki system me itni memory available hai ya nahi jisse naye process ka address space, stack, kernel stack aur management structures create kiye ja saken. Agar memory insufficient ho to process creation fail ho sakta hai.
    - Available PID : Har process ko ek unique Process ID (PID) chahiye hota hai. Kernel check karta hai ki naya unique PID available hai ya nahi. Agar PID allocation possible nahi ho to process create nahi ho sakta.
    - Security Policies : Kernel check karta hai ki system ki security policy, access-control rules ya protection mechanisms naye process ko create karne ki permission dete hain ya nahi. Agar security policy mana karti hai to process creation block kar diya jata hai.
    - Kernel resources : Kernel check karta hai ki uske paas process ko manage karne ke liye required internal resources available hain ya nahi, jaise PCB create karne ki memory, kernel data structures aur scheduling resources. Agar ye resources available nahi hain to process creation fail ho sakta hai.
2. Agr check fail ho gya to error return.
3. Agr Pass ho  gya to kernel naya process object create karne ka decision leta hai. 

## Phase 3 : PCB Memory Allocation
1. Kernel apni memory (kernel space) me PCB ka liye memory allocate karta hai.
2. PCB structure create hota hai.
3. Abhi PCB mostly empty hota hai.

## Phase 4 : Identity Information Fill
1. Kernel unique PID allocate karta hai.
2. Parent PID set karta hai.
3. User ID set karta hai.
4. Group ID set karta hai.
5. Process name set karta hai.
6. PCB ma identity section complete hota hai.

## Phase 5 : Process State Initialization
1. Process state initially new set hota hai.
2. scheduler related default values initialize hota hai.
3. priority initialize hota hai.
4. time accounting fields initialize hota hai.

## Phase 6 : Memory Information Attach 
1. Process ka lia address space create hota hai.
2. Page table structures create hota hai.
3. PCB ma page table pointer store hota hai.
4. Memory management information PCB ma attach hota hai.
5. Text, Data, Heap, Stack information PCB sa link hota hai.

## Phase 7 : CPU Context Setup 
1. Initial CPU context create hota hai.
2. Program counter initialize hota hai.
3. Stack point initialize hota hai.
4. Initial register values prepare hota hai.
5. Initial flags set hota hai.
6. Ya context PCB ma save hota hai.

## Phase 8 : Resource information Attach 
1. Open file table references attach hota hai.
2. Working directory information attach hota hai.
3. Signal handling information attach hota hai.
4. Security credentials attach hota hai.
5. Resource limits attach hota hai.

## Phase 9 : Scheduler Integration
1. Scheduler PCB ko register karta hai.
2. PCB ko system process list ma add kiya jata hai.
3. Ready queue insertion ki preparation hoti hai.
4. Process state New -> Ready hota hai.
5. PCB Ready Queue ma insert ho jata hai.

## Phase 10: PCB Fully Active
1. Ab PCB complete ho chuka hota hai.
2. OS process ko officially recognize karta hai.
3. Scheduler future ma is PCB ko select kar shakta hai.
4. Jab CPU milega to isi PCB se context resotre hoga.
5. Process running state me jayega. 
