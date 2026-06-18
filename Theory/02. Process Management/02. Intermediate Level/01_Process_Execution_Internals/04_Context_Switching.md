# 1. Exact Context Content
## 1.1 Exact Process Context Content
1. Program Counter - Next Instruction Kahan sa continue hogi.
2. CPU Register State - General purpose registers ka current values.
3. SP - Current stack position.
4. Flag Register - Zero, carry, sign, overflow etc.
5. Memory Context - Kaunsa address space/process memory active hai.
6. Process State - Running, Ready, Waiting, Etc. 

## 1.2 Exact Thread Context Content 
1. Program Counter → CPU ko batata hai ki thread ki next instruction kis address se execute karni hai.
2. CPU Registers → Thread ke execution ke dauran use ho rahe temporary data aur intermediate results ko store karte hain.
3. Stack Pointer → CPU ko batata hai ki thread ka current stack top kis address par hai.
4. Status Register → CPU ki current condition aur ALU operation ke flags ko store karta hai.
5. Thread State → Operating System ko batata hai ki thread Running, Ready, Waiting ya Terminated state me hai.
   
# 2. Detailed Internal Flow
## Phase 0 : Process A Running
1. Process A currently CPU par execute ho rahi hoti hai.
2. CPU ke Program Counter me Process A ki next instruction ka address hota hai.
3. CPU Registers me Process A ka temporary execution data hota hai.
4. Stack Pointer Process A ke stack ko point kar raha hota hai.
5. Status Register me Process A ke flags stored hote hain.
6. CPU normal Fetch → Decode → Execute cycle chala raha hota hai.

## Phase 1 : Context Switch Trigger
1. Ek event occur hota hai.
2. Ye event timer interrupt ho sakta hai.
3. Ya Process A I/O wait me ja sakti hai.
4. Ya higher priority process ready ho sakti hai.
5. Ya scheduler ko CPU re-allocation karna pad sakta hai.
6. Operating System decide karta hai ki ab context switch karna hai.

## Phase 2 : CPU Kernel Mode Me Enter Karta Hai
1. CPU currently user mode me Process A execute kar raha tha.
2. Interrupt ya trap receive hota hai.
3. CPU current user execution temporarily stop karta hai.
4. CPU privilege level change karta hai.
5. CPU user mode se kernel mode me switch karta hai.
6. CPU kernel ke interrupt handler ya scheduling code par control transfer karta hai.
7. Ab kernel code execute ho raha hai.

## Phase 3 : Current Context Capture Start
1. Kernel sabse pehle current CPU state ko preserve karna chahta hai.
2. Kernel CPU ke Program Counter ki current value read karta hai.
3. Kernel CPU Registers ki current values read karta hai.
4. Kernel Stack Pointer ki current value read karta hai.
5. Kernel Status Register ki current value read karta hai.
6. Kernel current execution state collect karta hai.

## Phase 4 : PCB Update
1. Kernel Process A ka PCB locate karta hai.
2. PCB Process A ka management record hota hai.
3. Kernel Program Counter value PCB me save karta hai.
4. Kernel Register values PCB me save karta hai.
5. Kernel Stack Pointer PCB me save karta hai.
6. Kernel Flags PCB me save karta hai.
7. Kernel Process A ki latest execution state PCB me update kar deta hai.
8. Ab Process A future me restore ki ja sakti hai.

## Phase 5 : Process State Change
1. Kernel Process A ki state evaluate karta hai.
2. Agar time quantum expire hua hai:
- Running → Ready
3. Agar I/O wait hua hai:
- Running → Waiting
4. Kernel PCB me state field update karta hai.
5. Scheduler future decisions ke liye updated state use karega.

## Phase 6 : Scheduler Execution
1. Ab current process save ho chuki hai.
2. Kernel scheduler ko invoke karta hai.
3. Scheduler ready queue inspect karta hai.
4. Scheduler scheduling algorithm apply karta hai.
5. Scheduler decide karta hai ki next CPU kisko milega.
6. Scheduler Process B select karta hai.

## Phase 7 : Next PCB Locate
1. Kernel Process B ka PCB locate karta hai.
2. PCB me Process B ki previously saved execution state hoti hai.
3. Kernel restore operation start karta hai.

## Phase 8 : Context Restore
1. Kernel PCB-B se saved Program Counter read karta hai.
2. Kernel PCB-B se saved Register values read karta hai.
3. Kernel PCB-B se saved Stack Pointer read karta hai.
4. Kernel PCB-B se saved Flag values read karta hai.
5. Kernel ye values CPU me load karta hai.
6. CPU Process B ka purana execution environment recreate kar leta hai.

## Phase 9 : Process State Update
1. Process B pehle Ready state me thi.
2. CPU allocation milne ke baad:
- Ready → Running
3. PCB me Process B ki state update ki jati hai.

## Phase 10 : Return To User Mode
1. Kernel ka scheduling work complete ho chuka hai.
2. CPU kernel mode se exit karne ki preparation karta hai.
3. CPU user mode restore karta hai.
4. CPU Program Counter ki restored value use karta hai.
5. CPU exactly us instruction se continue karta hai jahan Process B pehle ruki thi.

## Phase 11 : Execution Resume
1. CPU Process B ki next instruction fetch karta hai.
2. Decode karta hai.
3. Execute karta hai.
4. Process B ko lagta hai ki wo continuously chal rahi hai.
5. Use pata bhi nahi hota ki beech me context switch hua tha.

# 3. Context Switch Overhead
Context Switch Overhead woh extra CPU cost hai jo ek process/thread se dusre process/thread par switch karne ke liye context save, context restore aur scheduling activities me lagti hai. Ya isiliye hota hai qoki context switch free operation nahi hai CPU ko current process save karna parta hai, scheduler chalana parta hai, next process select karna parta hai, next process restore karna parta hai. 

