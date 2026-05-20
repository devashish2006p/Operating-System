# 1. Process Synchronization Tools

## 1.1 Atomic Operations 
1. **Test-and-Set** - Ya ek atomic hardware instruction hai jo ek hi step ma ksi lock ko check bhi karta hai aur ushko set bhi kar deta hai, taki race condition na ho aur mutual exclusion achieve ho sake.

- **Internal Mechanism**
- 1. CPU ek shared variable *lock* ko memory ma read karta hai aur current state check karta hai.
  2. Ya read operation ka sath hi CPU internally decide karta hai ki lock ko modify karna hai.
  3. Agr *lock = 0* (free) hai to CPU ushko *1* (busy) set kar deta hai.
  4. Ya read + set dono ek hi atomic instruction ma hota hai, beech ma koi interrupt nahi hota.
  5. Instruction ka result process ko return hota hai. (true/false ya old value).
  6. Agr process ko pta chalta hai ki lock pehle sa *1* hai to wo busy-wait loop me chala jata hai.
  7. Busy-wait ma process continuously *Test-and-Set* call karta rehta hai jab tak lock free na ho jaye.
  8. Jaisa hi lock free hota hai process ushko acquire karke critical section ma enter kar leta hai.
  9. Critical section complete hone ka baad process manually lock ko 0 (free) set karta hai.  

- **Advantages**
- 1. Ek time par sirf ek process cirtical section ma enter kar shakta hai.
  2. Lock checking aur setting atomic operation ma hota hai, isiliye conflict nahi hota.
  3. CPU instruction directly execute hoti hai, isiliye synchronization efficient hota hai.
  4. Sirf 2 nahi, bahut sare processes ek hi lock use kar shakta hai.
  5. Modern mutex/spinlock implementations ma ishka concept use htoa hai.

- **Disadvantages**
- 1. Waiting processes continously loop ma lock check karte rehta hai, CPU waste hota hai.
  2. Jo process favorable timing pa lock paa leta hai wahi enter karta hai.
  3. Koi process repeatedly lock miss karta rehe to indefinitely wait kar shakta hai.
  4. Waiting processces useful kaam nhi karte, sirf spinning karte rehte hai.
  5. Bahut sare processces same lock ka lia compete karein to overhead badh jata hai.
  
2. Compare-and-Swap - Ya ek atomic hardware instruction hai jo memory ki current value ko expacted value sa compare karta hai aur agr dono same ho to us value ko new value sa replace kar deta hai warna koi change nahi karta.

- **Internal Mechanism**
- 1. Process CPU ko ek CAS instruction deta hai jishme 3 cheeze hote hai.
     - Memory location
     - Expected value
     - New value
  2. CPU atomic operation start karta hai aur memory ki currect value read karta hai.
  3. CPU current value ko expected value sa compare karta hai.
  4. Agr current value == expected value hoti hai to CPU us memory location ma new value store kar deta hai.
  5. Agr current value != expected value hoti hai to CPU memory ko change nahi karta.
  6. Ya compare + update operation ek hi atomic step ma hota hai, beech ma koi interrupt ya dusra process interfere nahi kar shakta.
  7. Process ko operation ka result milta hai.
     - success -> value update ho gui.
     - fail -> value same rahi.
  8. Agr CAS fail hota hai, to process usually retry loop me dobara attempt karta hai.
  9. Successful CAS ka baad process lock/resource acquire karke critical section me enter kar shakta hai. 

- **Advantages**
- 1. Compare aur update ek hi uninterruptible operation ma hota hai isiliye race condition avoid hoti hai.
  2. Kuch data structures bina traditional locks ka safely implement kiya ja shakta hai.
  3. Low contention systems ma locking overhead kam hota hai.
  4. Modern multi core systems ma widely supported hota hai.
  5. Sirf lock management nahi, generic shared memory updates ka lia bhi use hota hai.
 
- **Disadvantages**
- 1. CAS fail hone par process repeatedly retry karta hai, CPU waste ho shakta hai.
  2. Bahut saare processes ek hi variable update karne ki koshish karein to retries bahut badh jate hai.
  3. Correct lock-free algorithms design karna difficult hota hai.
  4. Koi process repeatedly CAS fail karta rahe to long time wait kar shakta hai.
  5. Value temporarily change hokar wapas same ho jaye to CAS ko lag shakta hai kuch change nahi hua, jo logical bug create kar shakta hai.
     
3. Disable Interrupts - Ya ek hardware level synchronization technique hai jishme CPU temporarily interrupts ko band kar deta hai taki current process ko critical section execute karte waqt koi context switch ya interruption na ho, aur shared resource safely access ho sake. 

- **Internal Mechanism**
- 1. Jab process critical section ma enter karna cahta hai to OS/CPU insterrupt disable instruction execute karta hai.
  2. CPU internally interrupt flag ko OFF kar deta hai.
  3. Interrupt flag OFF hote hi
     - time interrupts
     - I/O interrupts
     - scheduler interrupts
     -> Temporary ignore hone lagte hai.
  4. Ab current process uninterrupted CPU execute karta rehta hai.
  5. Qoki scheduler interrupt trigger nhi ho pata to context switch nahi hota.
  6. Isiliye dusra process CPU acquire nahi kar pata aur critical section ma enter nahi kar shakta.
  7. Current process safely shared resource use karta hai aur apna critical kaam complete karta hai.
  8. Kaam complete hone ka baad CPU interrupt-enable instruction execute karta hai.
  9. CPU interrupt flag ko ON kar deta hai aur normal interrupt handling wapas start ho jata hai.
  10. Ab scheduler aur dusre processes firse normal execution continue kar shakta hai. 

- **Advantages**
- 1. Interrupts off hone par context switch nhi hota, isiliye ek time par sirf current process critical section execute karta hai.
  2. Dusra process CPU acquire nahi kar pata, shared resource safe rehta hai.
  3. Sirf interrupt disable/enable instructions ka use hota hai.
  4. Hardware-level operation hone ki wajah sa overhead kam hota hai.

- **Disadvantages**
- 1. Multi core systems ma dusre cores independently execute kar shakte hai.
  2. Interrupts off hone par important events temporarily handle nhi hote.
  3. Agr process zyada der interrupts disable rakhe to system lag/freeze jaisa behave kar shakta hai.
  4. Agr normal users interrupts disable kar sakein to pura system unstable ho shakta hai.
  5. Sirf interruption rokta hai, proper scheduling control nahi deta.
     
4. Fetch-and-Add - Fetch-and-Add ek atomic hardware instruction hai jo kisi shared variable ki current value ko pehle read (fetch) karta hai aur phir usme ek specified value add karke usko update kar deta hai, aur ye dono operations ek hi uninterruptible step me hote hain.

- **Internal Mechanism**
- 1. Process CPU ko Fetch-and-Add (variable, value) instruction deta hai.
  2. CPU atomic operation start karta hai aur shared variable ki current value memory sa read karta hai.
  3. CPU current value ko temporarily store karta hai aur old value preserve karta hai.
  4. CPU specified value ko current value ma add karta hai.
  5. CPU updated result ko wapas same memory location ma store kar deta hai.
  6. Read + add + update ya teeno kaam ek hi atomic instruction ma hota hai, isiliye beech ma koi dusra process interfere nahi kar shakta.
  7. CPU process ko old value return karta hai jo addition sa pehle variable me thi.
  8. Dusre processes agr same variable use karna cahta hai to unhe updated value milegi.
  9. Synchronization systems ma is old/new value ka use turn assignment, ticket generation, counters ya queue ka lia hota hai.

- **Advantages**
- 1. Read aur update ek hi uninterruptible operation ma hota hai, isiliye race condition avoid hoti hai.
  2. Multiple processes ko safely alag-alag ticket/counter values milti hai.
  3. Ticket locks aur queue based syncrhonization ma FIFO stype ordering possible hoti hai.
  4. Modern processors atomic fetch and add efficiently support karte hai.
  5. Shared counters concurrent enviroment ma safely update hota hai.

- **Disadvantages**
- 1. Busy waiting systems ma CPU waste ho shakta hai.
  2. Bahut sare processes same variable update karein to memory contention badh jata hai.
  3. Multi-core systems ma shared variable synchronize karne ka hardware cost badhta hai.
  4. Ya sirf atomic primitive hai, full locking policy seperately implement karni parti hai.
  5. Correct synchronization design carefully karna parta hai. 

## 1.2 Spinlock
Spinlock ek synchronization tool/mechanism hota hai jisme jab koi thread/process lock nhi paa pata, to wo sleep ya block hone ka bajay continuously loop me lock ko check karta rehta hai, aur jaisa hi lock free hota hai waisa hi ushe acquire karke critical section ma enter kar jata hai. 
- **Internal Mechanism**
- 1. System ek shared lock variable maintain karta hai.
  2. Jab thread critical section ma jana cahta hai to wo lock acquire karne ki koshish karta hai.
  3. Agr lock free hai to thread turant lock le leta hai aur critical section ma enter kar jata hai.
  4. Agr lock already ksi aur thread ka pass hai to waiting thread sleep nhi karta.
  5. Waiting thread continuously CPU par loop chalata rehta hai aur repeatedly lock check karta hai.
  6. Is continuous checking ko "spinning" Kehte hai.
  7. Jaisa hi current thread critical section complete karta hai wo lock release kar deta hai.
  8. Fir spinning threads ma sa koi ek thread lock acquire kar leta hai aur critical section me enter karta hai.

- **Advantages**
- 1. Short waiting time ma fast hota hai qoki sleep/wakeup overhead nahi hota.
  2. Context swtch avoid hota hai.
  3. Multi-core systems ma efficient ho shakta hai.
  4. Kernel/internal OS syncrhonization ma useful hota ha. 
- **Disadvantages**
- 1. Waiting thread continuously CPU consume karta rehta hai.
  2. Long critical section ma bahut CPU waste hota hai.
  3. Single core systems ma inefficient ho shakta hai.
  4. Starvation possibility ho shakti hai.
  5. High contention par performance degrade hoti hai. 
## 1.3 Mutex
Ya ek synchronization tool hai jo ensure karta hai ki ek time par sirf ek hi thread/process shared resource ya critical section ko access kare, taki race condition na ho aur data safe rahe. 
- **Internal Mechanism**
- 1. System ek mutex object maintain karta hai.
  2. Mutex ki state hoti hai:
     - unlocked (free)
     - locked (busy)
  3. Jab koi thread critical section ma jana cahta hai to wo mutex lock request karta hai.
  4. Agr mutex free hai to OS/CPU us thread ko mutex de deta hai.
  5. Ab mutex locked state ma chala jata hai.
  6. Dushre threads ab critical section ma enter nahi kar shakte.
  7. Agr koi dusra thread mutext acquire karne ki koshish karta hai to  wo usually sleep/block state ma chala jata hai.
  8. Current thread shared resource safely use karte hai.
  9. Kaam complete hone ka baad thread mutext unlock karta hai.
  10. Unlock hote hi OS waiting threads ma sa ksi ek ko mutex de deta hai.
  11. Fir next thread critical section execute karta hai. 
- **Advantages**
- 1. Race condition avoid karta hai.
  2. CPU waste kam hota hai qoki waiting threads sleep karte hai.
  3. Multi-threaded programming ma widely use hota hai.
  4. Shared data safely access hota hai. 
- **Disadvantages**
- 1. Context switching overhead hota hai.
  2. Galat use sa deadlock ho shakta hai.
  3. Lock contention zyada ho to waiting increase hoti hai.
  4. Incorrect unlock handling bugs create kar shakta hai. 
## 1.4 Semaphore
Ya ek synchronization tool hai jo multiple threads/processes ko shared resources ko controlled way ma access karne deta hai, jahan ek counter maintain hota hai jo decide karta hai kitne threads ek resource use kar sakte hai. 
- **Internal Mechanism**
- 1. System ek semaphore variable maintain karta hai.
  2. Semaphore ma ek integer counter hota hai.
  3. Jab koi thread resource use karna chahta hai to wo wait() operation call karta hai.
  4. OS semaphore counter check karta hai.
  5. Agr counter > 0 hai:
     - counter decrease hota hai.
     - thread ko entry mil jati hai.
  6. Agr counter == 0 hai:
     - resource available nahi hai.
     - thread wait/block state ma chala jata hai.
  7. Jab koi running thread apna kaam complete karta hai to wo signal() call karta hai.
  8. Signal operation counter increase karta hai.
  9. Agr koi waiting thread hai to OS unme sa ksi ek ko wake up karta hai.
  10. Fir awakened thread resource use karne start karta hai. 
- **Types of Semaphore**
- 1. Binary Semaphore : Ya ek aisa semaphore hota hai jiske counter sirf 0 ya 1 ho shakta hai, aur ishka use ek time par sirf ek hi thread/process ko shared resource access dene ka liye kiya jata hai. 
  2. Counting Semaphore : Counting semaphore ek aisa semaphore hota hai jisme counter multiple integer values hold kar shakta hai aur ishka use multiple identical resources ko manage karne ka lia kiya jata hai. 
- **Advantages**
- 1. Multiple resources manage kar shakta hai.
  2. Race condition avoid karta hai.
  3. Producer-consumer jaisi problems ma useful hai.
  4. Busy waiting avoid kar shakta hai.
  5. Thread coordination me useful hai.
- **Disadvantages**
- 1. incorrect use sa deadlock ho shakta hai.
  2. Debugging difficult hoti hai.
  3. Wrong signal/wait order bugs create kar shakta hai.
  4. Starvation possibility ho shakti hai. 
## 1.5 Condition Variable 
Condition variable ek synchronization tool hota hai jisme threads ksi specific condition/event ka true hone ka wait karte hai, aur jab condition satisfy hoti hai tab wating threads ko signal dekar wake up kiya jata hai.
Condition variable khud resource protect nahi karta balki usually mutex ka sath use hota hai. 
- **Internal Mechanism**
- 1. Threads ek shared condition check karta hai.
  2. Agr condition false hai to thread condition variable par wait karta hai.
  3. Wait karte waqt thread usually sleep/block state ma chala jata hai.
  4. Waiting ka time mutex temporarily release kar diya jata hai.
  5. Dusra thread shared data modify karta hai.
  6. Jab required condition ture ho jati hai to signaling thread signal() ya broadcast() bhejta hai.
  7. OS waiting threads ko wake up karta hai.
  8. Awakened thread mutext dobara acquire karta hai.
  9. Fir condition dobara check karke execution continue karta hai. 
- **Advantages**
- 1. Busy waiting avoid hoti hai.
  2. CPU waste kam hota hai.
  3. Efficient thread co-ordination hota hai.
  4. Producer-consumer systems ma useful hai. 
- **Disadvantages**
- 1. Mutex ka bina unsafe ho shakta hai.
  2. Incorrect signaling bugs create kar shakta hai.
  3. Deadlock possibility hoti hai agr misuse ho.
  4. Logic carefully design karna parta hai. 
