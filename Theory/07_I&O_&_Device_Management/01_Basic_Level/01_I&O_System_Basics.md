# 1. I/O System 
I/O System OS aur Computer Architecture ka woh part hota hai jo CPU, memory aur external devices (jaisa keyboard, mouse, disk, printer) ka beech data transfer aur communication ko controllers, interrupts aur DMA ka through efficiently aur safely manage karta hai. 

# 2. Types of I/O Devices 
## 2.1 Block Devices 
Block device woh I/O device hota hai jo data ko fixed-size blocks (chunks) me read/write karta hai aur jisme random access possible hota hai.

## 2.2 Character Devices 
Character device woh I/O device hota hai jo data ko continuous stream (byte-by-byte) form me read/write karta hai, bina fixed-size blocks ke.

## 2.3 Network Devices 
Network device woh I/O device hota hai jo computer ko network ke through dusre computers ya systems ke saath data packets ke form me communicate karne deta hai, jahan data OS network stack ke through send/receive hota hai.

## 2.4 Virtual/Pseudo Devices 
Virtual (ya pseudo) devices woh I/O devices hote hain jo real hardware nahi hote, balki OS kernel software ke through simulate karta hai taaki applications ko standard device-like interface mil sake.

# 3. Device Abstraction
Device Abstraction ek mechanism hai jo hardware ki complexity ko hide karke programmers aur applications ko devices use karne ke liye ek simple aur common interface provide karta hai.

  ## 3.1 Internal Core components 
  ### 1. Common Interface / System Call Interface 
  Ya OS ki standard entry layer hai jiske through applications I/O services request karti hai. 
  #### **Role**
  1. Applications ko OS ki I/O services access karne ka standard entry point provide karta hai.
  2. Programmers ko hardware specific commands sa bachata hai.
  3. Different devices ka lia ek common access method provide karta hai.
  4. User space aur kernel space ka beech controlled communication provide karta hai.
  5. Device abstraction ka first visible layer provide karta hai.
  6. Applications ki I/O requests ko OS tak pahunchata hai.
  7. Hardware complexity ko applications sa hide karta hai. 
    
  ### 2. Device Independent OS Layer / Kernel I/O Subsystem
  Ya OS ki internal management layer hai jo I/O requests ko process karti hai aur device independent services provide karti hai.  
  #### **Role**
  1. System call sa aayi I/O requests ko process aur manage karta hai.
  2. Different devices ka liya common I/O services provide karta hai.
  3. Appropriate device driver ko select aur request forward karta hai.
  4. Buffering manage karta hai taki I/O performance improve ho.
  5. Chaching manage karta hai taki repeated access fast ho sake.
  6. I/O request queues ko manage karta hai.
  7. Error detection aur handling perform karta hai.
  8. Access permissions aur protection enforce karta hai.
  9. Device specific details ko baaki OS aur aplications sa hide karta hai.
  10. I/O resources ko efficiently manage aur optimize karta hai
        
  ### 3. Device Drivers 
  Ya ek software component hota hai jo Operating System aur specific hardware device ka beech communication karwata hai aur OS ki requests ko device specific commands ma convert karta hai. 
  #### **Role**
  1. OS aur hardware device ka beech communication karwata hai.
  2. OS ki requests ko device specific commands ma convert karta hai.
  3. Device ka response ko OS ka understandable format ma convert karta hai.
  4. Hardware specific details ko OS sa hide karta hai.
  5. Device controller ka registers ko access karta hai.
  6. Device initialization aur configuration karta hai.
  7. Interrupts aur device events handle karta hai.
  8. Device ka read/write operations ko control karta hai. 
  ### 4. Hardware/ Device Controller Layer 
  Ya ek physical hardware layer hoti hai jo actual device ko control karti hai aur device driver sa aayi commands ko hardware level operations ma convert karti hai. 
  #### **Roles**
  1. Actual hardware device ko control karta hai.
  2. Driver sa aayi commands ko execute karta hai.
  3. Data transfer operations perform karta hai.
  4. Device ki status information maintain karta hai.
  5. Interrupt generate kar shakta hai jab operation complete ho.
  6. Control, status aur data registers provide karta hai.
  7. CPU aur device ka beech low-level communication handle karta hai.
  
  ## 3.2 Internal Mechanism
  1.Application kisi device par operation perform karne ke liye I/O request generate karti hai.

  2. Request Common Interface/System Call Interface ke through OS me enter karti hai.
  
  3. System Call Interface request ko Kernel I/O Subsystem tak pahunchata hai.
  
  4. Kernel I/O Subsystem request ko receive karke validate aur process karta hai.
  
  5. Kernel I/O Subsystem identify karta hai ki request kis device ke liye hai.
  
  6. Kernel I/O Subsystem appropriate Device Driver ko request forward karta hai.
  
  7. Device Driver OS ki generic request ko device-specific commands me convert karta hai.
  
  8. Device Driver device controller ke registers aur control mechanisms ko access karta hai.
  
  9. Device Controller driver se aayi commands ko actual hardware operations me convert karta hai.
  
  10. Hardware device requested operation (read/write/send/receive) perform karta hai.
  
  11. Operation complete hone par device controller status update karta hai aur zarurat padne par interrupt generate karta hai.
  
  12. Device Driver operation result aur status ko OS ke understandable format me convert karta hai.
  
  13. Kernel I/O Subsystem result ko process karke application ke liye prepare karta hai.
  
  14. System Call Interface result ko application tak return kar deta hai.
  
  15. Application ko device operation ka final result mil jata hai, bina hardware ki internal complexity jaane.
      
# 4. Device Controller 

# 5. Memory Vs Device Concept 
