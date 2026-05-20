# 1. Process Synchronization
Process synchronization ek aisa system control hai jo multiple processes ko manage karta hai taki jab wo ek hi cheez (resource) use karein, to na takraayein, na data kharab ho, aur sab ka kaam sahi order me aur safely ho.

# 2. Shared Vs Non-shared resources
1. Shared resources : Shared resources wo hote hain jinko ek se zyada processes ek hi time ya alag-alag time par use kar sakte hain.
2. Non-shared resources : Non-shared resources wo hote hain jo sirf ek hi process ke liye hote hain aur unhe dusre processes access nahi karte.

# 3. Concurrency Vs Parallelism
1. Concurrency : Concurrency ka matlab hai system ka multiple tasks ko ek hi time period me manage karna, jahan CPU unke beech fast switching karta hai, isliye hume lagta hai ki sab kaam ek saath chal rahe hain, chahe actually wo ek-ek karke execute ho rahe ho.
2. Parallelism : Parallelism ka matlab hai system ka multiple tasks ko literally ek hi exact time par execute karna, jahan har task ke liye alag CPU core ya processor use hota hai

# 4. Processes Vs Threads
1. Process - Process ek running program hota hai jise OS execute kar raha hota hai, jiske paas apna alag memory space, system resources (jaise CPU time, files, I/O), aur execution state hoti hai, aur jo dusre processes se independent tarike se chalta hai.

2. Thread - Thread ek process ke andar chalne wala lightweight execution unit hota hai jo CPU par instructions execute karta hai, aur process ki memory aur resources ko dusre threads ke saath share karta hai, lekin apna execution flow aur stack alag rakhta hai

# 5. Core Problems of Process Synchronization
1. Race Condition - Race condition ek aisi situation hoti hai jahan do ya usse zyada processes ek hi data ko bina proper control ke ek saath access ya change karte hain, jiski wajah se final result galat ya unpredictable ho jata hai, kyunki system ko ye control nahi hota ki kaun pehle aur kaun baad me execute hoga.
2. Critical Section Problem - Critical section problem ek aisi situation hai jahan multiple processes ek hi shared resource ko use karne ke liye program ke us part me enter karne ki koshish karte hain, aur agar unhe control na kiya jaye to wo ek saath enter karke data ko galat ya corrupt kar dete hain, isliye zaroori hota hai ki ek time par sirf ek hi process us critical section me jaye.
3. Deadlock - Ya ek aisi dangerous situation hoti hai jahan do ya zyada processes alag-alag resources ko hold karke baith jate hain aur ek dusre ke paas jo resource hai uska wait karte rehte hain, jisse koi bhi process aage nahi badh pata aur system practically atak jata hai
4. Starvation - Starvation wo situation hoti hai jahan koi process baar-baar ignore hota rehta hai aur usse resource ya CPU use karne ka chance nahi milta, kyunki system baar-baar dusre processes ko priority de deta hai, aur wo process indefinitely wait karta rehta hai.
5. Livelock - Ya ek aisi situation hai jahan do ya zyada processes ek dusre ke saath continuously adjust ya react karte rehte hain (jaise baar-baar side dena), isliye wo block nahi hote lekin actual kaam kabhi aage nahi badh pata.

# 6. Software solutions
1. **Locking** - Locking ek aisa mechanism hai jisme ek process resource par control (lock) laga deta hai taki jab tak wo kaam kar raha ho, koi dusra process us resource ko access na kar sake.

**Core Components for Locking**
1) Lock (state) - Lock state ek indicator hota hai jo btata hai ki resource abhi free hai ya ksi process ka control ma hai.
2) Acquire (Lock lena) - Acquire wo process hai jishme koi process check karta hai ki lock free hai ya nahi, aur agr free ho to usshe le leta hai, warna wait karta hai.
3) Release (Lock chhodna) - Ya wo process hai jisme process apna kaam khatam karke lock ko free kar deta hai tak idusre processes us resource ko use kar sakein. 

**Internal Mechanism**
1) System ek lock variable maintain karta hai jo btata hai ki resource free hai (0) ya busy (1).
2) Jab koi process resource use karna chahta hai to wo "acquire" operation karta hai mtlb wo check karta hai ki lock free hai ya nahi.
3) Agr lock free ho (0) tb process turant lock le leta hai aur 1 set kar deta hai and critical section ma enter karta hai.
4) Agr lock busy ho to (1) process resource use nahi kar shakta, wo ya to wait karega ya bar-bar check karega.
5) Jab process apna kaam complete kar leta hai tb wo "release" operation karta hai.
6) Release me process lock ko free(0) kar deta hai, mtlb ab resource dusre process kal lia available hai.
7) Ab jo process wait kar rhe the unme sa koi ek next lock acquire karega aur same cycle repeat hoti rhegi. 

2. **Strict Alternation** - Ya ek aisa algorithm hai jisme do processes ko ek shared resource use karne ka lia fixed turn diya jata hai, jishme wo ek ke bad ek hi enter kar shakte hai. 

- **Internal Mechanism**
- 1. System ma 2 pocessces hote hai jo ek shared resource use karna chahte hai.
  2. Ek shared control variable hota hai ho decide karta hai abhi kis process ko enter karna hai.
  3. Initially veriable ksi ek process ko assign hota hai (man lo P0).
  4. P0 critical section ma enter karne sa pehle check karta hai ki variable == 0 hai ya nahi.
  5. Agr variable == 0 hai to P0 critical section ma enter karta hai aur shared resource use karta hai. Agr nahi hai to P0 continuously wait karta hai jab tak turn change na ho.
  6. Critical section ka kaam complete hone ka baad P0 variable == 1 set karta hai mtlb ab P1 ka chance hai.
  7. P1 same process follow karta hai wo check karta hai variable == 1 hai ya nahi, agr hai to enter krega otherwise wait karega.
  8. P1 ka kaam complete hone ka baad wo variable == 0 set karta hai aur control wapas P0 ko de deta hai.

- **Advantages**
  1. Simple design hota hai qoki ek he shared variable use hota hai.
  2. Mutual exclusion gurantee karta hai ek time par sirf ek hi process critical section ma enter karta hai.
  3. Race condition avoid karta hai qoki entry strictly controlled hoti hai.
  4. Implementation easy hoti hai qoki ishme complext logic ya hardware support ki zarurat nhi hoti.

- **Disadvantages**
  1. Unnecessary waiting hoti hai, agr ek process kaam nhi bhi kar rha ho, to fir v dushre process ko wait karna parta hai.
  2. CPU time waste hota hai qoki busy waiting (loop ma wait karna) hota hai.
  3. Flexibility nahi hoti qoki order fixed hota hai aur dynamic scheduling possible nhi hota hai.
  4. Real OS ma use nahi hota qoki modern systems ma batter solutions available hai.
     
3. **Peterson's Algorithm** - Ya ek aisa software-based synchronization method hai jisme ki ek hi time par sirf ek process ko critical section me enter karne diya jata hai aur dusre process ko tab tak bahar rakha jata hai jab tak pehla process ka kaam complete nahi ho jata, isse shared data safe rehta hai aur race condition avoid hoti hai.
- **Internal Mechanism**
- 1. System ma sirf 2 processes hote hai P0 and P1 jo same shared resource use karna cahte hai.
  2. Do shared variables hote hai:-
     - flag[0] aur flag[1] -> btate hai ki process enter karna cahta hai ya nahi.
     - turn -> decide karta hai kisi priority hai agr dono ready ho.
  3. Jab process 0 ko critical section ma jana hota hai to wo fla[0] = true set karta hai.
  4. Ushke baad process 0 turn = 1 set karta hai mtlb "agr conflict hua to P1 ko priority do".
  5. Ab process 0 wait karta hai jab tak condition ture na ho.
     - flag[1] == false ya turn == 0
  7. Agr process 1 already critical section ma nahi jana cahta to process 0 direct enter kar jata hai.
  8. Agr process 1 bhi enter karna cahta hai to control turn variable decide karta hai ki kaun wait krega.
  9. Jab process ka kaam complete hota hai to wo apna flag false kar deta hai (flag[i] = false).

- **Advantages**
- 1. Ek time par sirf ek process cirtical section ma enter karta hai isiliye race condition nahi hoti.
  2. Koi special hardware support ya CPU instruction ki zarurat nhi hoti.
  3. Agr implement sahi ho to system deadlock ma nhi fasta.
  4. flag aur turn sa behavior easy samajh aata hai.

- **Disadvantages**
- 1. Process continuously loop ma wait karta rehta hai, CPU time waste hota hai.
  2. Multi process systems ka lia directly scalable nahi hai.
  3. Real systems me mutex/semaphore jaisa efficient tools use hote hai.
  4. Modern Compilers memory ordering ko change kar sakte hai, jissa correctness risk ma aa shakta hai.
  5. Ya assume karta hai ki read/write operations atomic aur ordered hai.
     
4. **Dekkers Algorithm**
Ya algorithm v Petersons's Algorithm ka trah he hai, intermal mechanism wgera v almost same he hai ishme sirf conflict handling thora alag hai qoki ishme conflict ma process temporary give up karne ka bad fir re-check+retry loop chalta hai aur thora complex/iterative flow hota hai.

6. **Bakery Algorithm**
Ya multiple processes ka beech mutual exclusion ensure karta hai jishme har process ek 'number' leta hai jise token kehte hai aur smallest number wale process ko critical section ma pehle entry milti hai.

- **Internal Mechanism**
- 1. System ma multiple processces hote hai jo same critical section share karte hai.
  2. Har process ka pass do shared arrays hote hai:-
     - choosing[i] -> ya btata hai process number select kar rha hai ya nahi.
     - number[i] -> ticket number store karta hai.
  3. Jab process i critical section jana cahta hai to wo choosing[i] = true set karta hai.
  4. Process i sbse bada current number check karta hai aur apna number set karta hai. "number[i] = max(number[all]) + 1".
  5. Fir choosing[i] = false kar deta hai (number assign ho gya).
  6. Process i har dusre process j ka liye check karta hai. Jab tak j ka choosing ture hai wait karta hai.
  7. Fir check karta hai agr number[j] != 0 && (number[j], j) < (number[i], i) hai to wait karta hai.
  8. Jab koi process j usshe "smaller ticket" nahi rakhta, tab process i critical section ma enter karta hai.

- **Advantages**
- 1. Har process ko ticket number milta hai, isiliye koi process unfairly skip nahi hota.
  2. Chuke har process ko eventually smallest number milne ka chance milta hai to starvation avoid hota hai.
  3. Ya sirf 2 processes ka lia nahi N processes ka lia kaam karta hai.
  4. Ksi special hardware instruction ki zarurat nahi hoti.

- **Disadvantages**
- 1. Process continuously loop ma check karta rehta hai, CPU waste hota hai.
  2. Har process ko max number find karna parta hai.
  3. Processes badhne par comparision cost increase ho jati hai.
  4. Sab processces ko shared variables access karne parte hai, jo complex system design banata hai. 

# 7. Hardware Solutions
1. **Test-and-Set** - Ya ek atomic hardware instruction hai jo ek hi step ma ksi lock ko check bhi karta hai aur ushko set bhi kar deta hai, taki race condition na ho aur mutual exlucsion achieve ho sake.

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
