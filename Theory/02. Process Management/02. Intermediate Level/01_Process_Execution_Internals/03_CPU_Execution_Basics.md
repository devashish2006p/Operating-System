# 1. CPU Execution 
CPU Execution woh process hai jisme CPU memory se instruction fetch karta hai, uska meaning decode karta hai, usko execute karta hai aur result ko register ya memory me store karta hai. 
# 2. Internal Core Components 
1. Clock - Clock ek hardware timing generator hai jo CPU ke sabhi internal operations ko synchronize karne ke liye regular electrical pulses generate karta hai, jinke basis par CPU ke components step-by-step kaam karte hain. Ishka kaam hai sare components ko timing provide karna aur CPU ko btana ki next operation kab karna hai.

2. Program Counter (PC) - Program Counter ek special register hai jo memory me next execute hone wali instruction ka address store karta hai taaki CPU ko pata rahe ki agali instruction kahan se fetch karni hai.

3. Memory Address Register (MAR) - Memory Address Register ek special register hai jo us memory location ka address temporarily hold karta hai jahan se CPU ko data ya instruction read karni hoti hai ya jahan data write karna hota hai.

4. Address Bus - Address Bus communication lines ka set hai jo CPU se memory ya I/O devices tak address pahunchata hai taaki required memory location select ki ja sake.

5. Main Memory  (RAM) - Main Memory ek volatile storage area hai jahan currently running programs ki instructions aur data temporarily store rehte hain aur jahan se CPU execution ke dauran information access karta hai.

6. Data Bus - Data Bus communication lines ka set hai jo CPU aur memory ke beech actual data aur instructions transfer karta hai.

7. Memory Data Register (MDR) - Memory Data Register ek temporary register hai jo memory se read kiye gaye data ya memory me write kiye jane wale data ko hold karta hai taaki CPU aur RAM ke beech data transfer properly ho sake. Ya data buffer ki trah kaam karta hai.

8. Instruction Register (IR) - Instruction Register ek special register hai jo currently execute hone wali instruction ko temporarily store karta hai taaki CPU uska analysis aur execution kar sake.

9. Control Unit (CU) - Control Unit CPU ka coordination center hai jo instruction ko decode karta hai aur execution ke liye CPU ke sabhi components ko appropriate control signals bhejkar unke operations ko manage karta hai.

10. Register Set (General Purpose Registers) - Register Set CPU ke andar maujood high-speed storage locations ka collection hai jo execution ke dauran operands, addresses aur intermediate results ko temporarily store karta hai.

11. ALU (Arithmetic Logic Unit) - Arithmetic Logic Unit CPU ka computational component hai jo arithmetic operations (addition, subtraction, multiplication, division) aur logical operations (AND, OR, NOT, comparison) perform karta hai.

12. Status/Flag Register - Status Register ek special register hai jo ALU operations ke results se related status information jaise Zero Flag, Carry Flag, Sign Flag aur Overflow Flag store karta hai taaki CPU future decisions aur branch instructions execute kar sake.

# 3. Deep Internal Workflow
## **Part 1 : CPU Execution Process**
### **Phase 0 : Program is on the Disk**
1. Program ek file ke roop me disk par pada hai. Joki abhi ye process nahi hai balki sirf ek executable file hai.
2. User Command deta hai executable file ko run karne ka aur shell ko input milta hai.
3. Shell Command Parse karti hai. Ya identify karta hai ki kaunsa executable chalana hai, arguments kya hai, enviroment variables kya hai.
4. Shell Kernel sa request karti hai. Linux ma commonly fork() and execve() path use hota hai.
5. CPU shell ka code execute kar rha hota hai aur shell user mode ma chal rhi hoti hai.
6. Shell system call instruction execute karta hai.
7. CPU privilege level change karta hai (User mode -> Kernel mode).
8. CPU kernel ka system call handler par jump karta hai. Ab kernel code execute ho rha hota hai.

### **Phase 1 : Process Creation**
9. Kernel request verify karta hai aur check karta hai ki permission hai ya nahi, file exist karti hai ya nahi, file executable hai ya nahi.
10. Kernel executable file locate karta hai aur file system use hota hai. 
11. File open hoti hai aur kernel executable ka content padhna shuru karta hai.
12. Executable header read hota hai jishke ander kernel ko pta chalta hai architecture, entry point, segment information etc.
13. Kernel executable verify karta hai jishme check karta hai corrupt to nahi hai file, architecture compatible to hai and etc.
14. Kernel PCB create karta hai aur aab jake process officially exist karna start karta hai.
15. PID allocate hota hai.
16. Kernel scheduler related information initialize karta hai.
17. Kernel Process Address Space create karta hai.
18. Virtual Address Space layout prepare hota hai.
19. Text segment map hota hai jaha par program instructions hote hai.
20. ROData segment map hota hai.
21. Data Segment map hota hai.
22. BSS segment map hota hai.
23. Heap region create hota hai.
24. Stack region create hota hai.
25. Shared libraries map hoti hai.
26. Memory mapped regions initialize hota hai.
27. Kernel stack create hota hai joki user stack sa alag hota hai.
28. Page table structures create hota hai.
29. Memory mappings setup hoti hai.

### **Phase 2 : Initial CPU Context Creation**
30. Kernel initial CPU context create karta hai.
31. Kernel executable header sa Entry point nikalta hai.
32. Initial PC value set hoti hai. (Jo kernel entry point nikala hota hai wo pc ko milta hai).
33. Initial Stack Pointer set hota hai.
34. Initial registers initialize hota hai.
35. Initial flags initialize hota hai.
36. Ya sari information PCB ma save hota hai.
37. Process State : New
38. Process Ready queue ma insert hoti hai.
39. Process State : Ready

 ### **Phase 3 : CPU Allocation**
 40. Scheduler Ready Queue inspect karta hai.
 41. Scheduler process select karta hai.
 42. Context switch start hota hai.
 43. Kernel PCB sa context read karta hai.
 44. Saved PC read hota hai.
 45. CPU ka PC register ma load hota hai.
 46. Registers restore hote hai.
 47. Flags restore hota hai.
 48. Address Space activate hota hai.
 49. Context restore complete.
 50. CPU process execute karne ka lia ready hai. Current State : Running

### **Phase 4 : First Instruction Fetch**
51. Ab PC ma entry point address aa chuka hota hai.
52. CU fetch cycle start karti hai.
53. CU Signal generate karti hai ki PC read karo.
54. PC ka value internal CPU datapath par aata hai.
55. Value MAR ma load hota hai.
56. CU memory Read signal generate karti hai.
57. MAR ka address Address Bus par jata hai.
58. RAM requested address identify karti hai.
59. Address par stored machine instruction locate hoti hai.
60. Instruction RAM sa Data Bus par aati hai.
61. Data MDR ma load hota hai.
62. MDR sa instruction IR ma jati hai.
63. IR ma current instruction aa chuki hai.
64. Fetch Complete. 

