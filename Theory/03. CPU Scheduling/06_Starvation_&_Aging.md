# 1. Starvation 
Starvation tab hota hai jab koi process lambe time tak CPU nahi paa pata qoki higher priority wale processes bar-bar execute hote rehte hai. Is situation ma low priority process continuously wait karta rehta hai aur ushko execution ka chance nhi milta. 

## 1.1 Reasons of Starvation
1. High priority processes ka domination - High priority wale processes bar-bar aate hai aur CPU le leta hai.
2. Strict priority scheduling - System hamesa highest priority ko hi select karta hai jiske karan low priority processes ignore ho jate hai.
3. Continuous arrival of new processes - Naye high priority processes lagatar aate rehte hai.
4. Improper scheduling policy - Agr scheduling algorithm fairness maintain nahi karta.

# 2. Aging
Ya **Starvation** ka ek main solution hai jo wait kar rhe processes ka priority ko dheere-dheere badhata hai, jisshe starvation kam ho jata hai. 

## 2.1 Aging Mechanism
1. Jab koi process ready state me aata hai, usko ek initial priority di jati hai.
2. Agar wo process CPU nahi paa pata, to wo runqueue me wait karta rehta hai.
3. System har process ka waiting time track karta rehta hai.
4. Jaise-jaise waiting time badhta hai, system us process ki priority dheere-dheere increase karta hai.
5. Ye priority increase ek fixed interval par hota hai (e.g., har kuch time units ke baad).
6. Priority badhne ke baad wo process scheduler ke liye zyada important ban jata hai.
7. Ek point par uski priority itni badh jati hai ki wo high priority processes ke equal ya unse zyada ho jata hai.
8. Scheduler fir us process ko select karta hai aur CPU de deta hai.
9. Process run hone lagta hai aur starvation khatam ho jata hai.
10. Execution ke baad uski priority normal ya reset ho sakti hai (system policy ke hisaab se).
