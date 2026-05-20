# 1. Deadlock Introduction
Deadlock ek aisi dangerous situation hoti hai jahan do ya usse zyada process/threads kuch resources ko hold karke baith jate hai aur sath hi unhe aage badhne ka lia dusre resources ki zarurat hoti hai jo ksi aur waiting process ka pass hote hai, isiliye sab ek dusre ke indefinitely wait karte rehte hai aur koi bhi process execution continue nhi kar pata. 
- **Effects of Deadlock**
  - 1. System hang/stuck ho shakta hai.
    2. Processes indefinitely wait karte hai.
    3. Resource utilization waste hota hai.
    4. Throughput reduce hota hai.
    5. System responsiveness kharab hoti hai.

# 2. Resource Concepts
Resource ek aisi system entity/object/service hoti hai jise processes ya threads apna kaam execute karne ka lia use karte hai, aur jo limited quantity me available hoti hai, isiliye multiple processes ka beech ushke liya competition ho shakta hai. 

## 2.1 Types of Resources
### 1. Reusable Resource
Reusable resource ek aisa resource hota hai jise ek process use karne ka baad release kar deta hai aur fir wahi same resource dusra process dobara use kar shakta hai, yani resource consume ya permanently khatam nhi hota. 

### 2. Consumable Resources
Consumable resource ek aisa resource hota hai jo use hone ka baad consume/finish ho jata hai aur fir wahi exact instance dobara reuse  nahi kiya jaa shakta, isiliye usually ushe producer ko firse generate/create karna parta hai. 
