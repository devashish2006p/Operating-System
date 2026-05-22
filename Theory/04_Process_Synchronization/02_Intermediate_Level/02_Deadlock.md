# 1. Deadlock Introduction
Deadlock ek aisi dangerous situation hoti hai jahan do ya usse zyada process/threads kuch resources ko hold karke baith jate hai aur sath hi unhe aage badhne ka lia dusre resources ki zarurat hoti hai jo ksi aur waiting process ka pass hote hai, isiliye sab ek dusre ke indefinitely wait karte rehte hai aur koi bhi process execution continue nhi kar pata. 
- **Effects of Deadlock**
  - 1. System hang/stuck ho shakta hai.
    2. Processes indefinitely wait karte hai.
    3. Resource utilization waste hota hai.
    4. Throughput reduce hota hai.
    5. System responsiveness kharab hoti hai.
       
--- 

# 2. Resource Concepts
## 2.1 About Resource
Resource ek aisi system entity/object/service hoti hai jise processes ya threads apna kaam execute karne ka lia use karte hai, aur jo limited quantity me available hoti hai, isiliye multiple processes ka beech ushke liya competition ho shakta hai. 

## 2.2 Types of Resources
### 1. Reusable Resource
Reusable resource ek aisa resource hota hai jise ek process use karne ka baad release kar deta hai aur fir wahi same resource dusra process dobara use kar shakta hai, yani resource consume ya permanently khatam nhi hota. 

- **Types of Reusable Resource**
- 1. Preemptable : Ya ek aisa reusable resource hota hai jise OS/process se zabardasti temporarily wapas le shakta hai bina system ko corrupt ya permanently damage kiye aur baad ma us resource ko firse allocate kiya ja shakta hai. Ex - CPU, RAM pages/ Memory frames, CPU registers, cache blocks, virtual memory space etc. 
  2. Non- preemptable : Ya ek aisa reusable resource hota hai jisa process se forcefully wapas nhi liya ja shakta bina data corruption, inconsistency ya system problem create kiye, isiliye process ko kaam complete karke khud resource release karna parta hai. Ex - Printer, Scanner, File lock, Database lock etc. 

### 2. Consumable Resources
Consumable resource ek aisa resource hota hai jo use hone ka baad consume/finish ho jata hai aur fir wahi exact instance dobara reuse  nahi kiya jaa shakta, isiliye usually ushe producer ko firse generate/create karna parta hai. Ex - Messages, Signals, Packets, Interrupts etc.  

## 2.3 Shared Vs Non Shared Resources 
- **Shared Resource** : Shared resource ek aisa resource hota hai jise ek sa zyada processes/threads controlled manner ma access ya use kar shakte hai.
- **Non-shared Resource** : Non-shared resource ek aisa resource hota hai jo ek time par sirf ek hi process/thread ka exclusion control/use me hota hai aur dusre processes ushe simultaneously access nhi kar shakte. 

---

# 3. Deadlock Conditions 
Deadlock conditions wo necessary/system conditions hoti hai jo agar ek sath system ma present ho jayein, to deadlock occur hone ki posibility create ho jati hai. 
1. Mutual Exclusion - Ya ek aisa condition hoti hai jisme ksi non-shared resource ya critical section ko ek time par sirf ek hi process access kar shakta hai aur dusre processes ko wait karna parta hai jab tak current holder ushe release na kare.

2. Hold and Wait - Ya ek aisi deadlock condition hoti hai jahan koi process ek ya zyada resources ko already hold karke rakhta hai aur sath hi additional resources ka liya wait bhi karta rehta hai jo currently dusre processes ka pass hote hai. 

3. No - Preemption - Ya ek aisi deadlock condition hoti hai jahan allocated non-preemptable resources ko process sa forcefully wapas nhi liya ja shakta aur process ko kaam complete karke khud voluntarily resource release karna parta hai. 

4. Circular wait - Ya ek aisi deadlock condition hoti hai jahan do ya usse zyada processes/resources ek circular chain ma ek dusre ka wait karte rehte hai, jisshe koi bhi process execution continue nhi kar pata. 

# 4. Resource Allocation Graph (RAG)
Resource Allocation Graph (RAG) ek graphical representation hota hai jo show karta hai kaunsa process kaunsa resource use kar rha hai aur kaunsa resource resource ka wait kar rha hai. RAG ka main kam sirf resource dependency ko visually dikhana  hota hai. 
## 4.1 Internal Components
1. Process Nodes - Ya ek graphical component hota hai jo system ka ksi process/thread ko represent karta hai, yani graph ma dikhata hai ki kaunsa process resources hold ya request kar rha hai. Ishka representation shape circle ka form a hota hai. 

2. Resource Nodes - Ya system ka ksi resource/resource type ko represent karta hai, yani graph ma dikhata hai ki kaunsa resource available, allocated ya requested hai. Ishka representation shape rectangle/box ka shape ma hota hai. 

3. Request Edge - Ya ek directed arrow hota hai jo process node sa resource node ki taraf jata hai aur ye represent karta hai ki process us resource ko request/wait kar rha hai. 

4. Assignment Edge - Ya ek directed arrow hota hai jo resource node sa process node ki taraf jata hai aur ye represent karta hai ki resource currently us process ko allocated hai.

# 5. Cycle Detection
Cycle detection ek analysis technique hota hai jisme Resource Allocation Graph (RAG) ma circular dependency/path ko idenfity kiya jata hai taki deadlock posibility ya actual deadlock ko detect kiya ja sake. 
