# 1. Definition 
Ya OS ka wo component hota hai jo scheduler ka decide kiya gye process ko actual ma CPU par run karwata hai. Ya current process ko stop karta hai, next process ka context load karta hai aur CPU ko us process ka execution ka lia set karta hai. 

# 2. Mechanism 
1. Scheduler decide karta hai ki kaunsa next process CPU par run karega.
2. Dispatcher ko ye decision milta hai ki “ab process switch karna hai”.
3. Dispatcher current running process ka context (registers, program counter, stack) save karta hai.
4. Saved context ko us process ke PCB (Process Control Block) me store kar diya jata hai.
5. Dispatcher next process ka PCB uthata hai jise run karna hai.
6. Us process ka saved context (registers, PC, stack) load kiya jata hai.
7. CPU ke registers aur state ko naye process ke according set kiya jata hai.
8. Memory mapping (address space) bhi naye process ke hisaab se switch hoti hai.
9. CPU control officially naye process ko de diya jata hai.
10. Naya process wahi se execution continue karta hai jahan wo last time rukha tha.

# 3. Dispatcher Latency 
Ya wo time hota hai jo dispatcher ko ek process sa dusre process par switch karne ma lagta hai. 

## 3.1 Reasons of Dispatcher Latency
1. Current process ka data save karna padta hai.
2. Next process ka data load karna padta hai.
3. CPU registers change karne padte hai.
4. Memory mapping switch hoti hai.
5. Kernel mode sa user mode ma jana padta hai.
6. Cache/TLB refresh ya effact hota hai.
7. Protection & security checks hote hai.

## 3.2 Ways of optimize dispatcher latency
1. Lightweight context switch
2. Bar-bar process swtich ko avoid karna.
3. Same process ko thoda zyada time dena.
4. Bahut chota time slice avoid karna cahiya. 
