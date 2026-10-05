# 1. Segmentation
Segmentation ek memory-management technique hai jisme process ke address space ko alag-alag logical parts, yani segments, mein divide kiya jata hai—jaise Code, Heap aur Stack. Har segment ka apna Base aur Bounds register hota hai, jisse OS segments ko physical memory mein alag-alag jagah rakh sakta hai.

# 2. Memory Organization
## 2.1 Physical Memory Organization
Segmentation physical RAM ko kisi special structure mein organize nahi karti. Physical memory khud basically ek linear array of physical addresses hoti hai. Segmentation ka effect ye hota hai ki OS us RAM mein variable-size contiguous regions allocate karke unhe different logical segments ke saath associate karta hai.

## 2.2 Virtual Memory Organization
Virtual memory mein process ko ek single continuous usable region ki tarah nahi, balki multiple logical segments ki tarah organize kiya jata hai.
  - **Common Types of Segments**
    1. Code/Text Segment - Program ki executable instruction hoti hai jishko normally read, execute ya write nahi kar shakte.
    2. Data Segment - Initialized global aur static variables rakhe jate hai.
    3. BSS Segment - Uninitialized ya zero initialized global/static variables.
    4. Heap Segment - Runtime par dynamically allocated memory, jaisa malloc() se.
    5. Stack Segment - Function calls, local variables, return information stack frames etc.
    6. Read Only Data/RO Data - Constrants aur read-only data, jaisa strong literals hote hai.
    7. TLS (Thread-local storage) - Har thread ki apni private variables.
    8. shared libraries/objects - Dynamically loaded libraries ki mapped memory.
    9. Memory Mapped regions - Files, shared memory, anonymous mappings aur other mmap() based regions.
   
# 3. Memory Tracking
Memory Tracking ka mtlb hota hai OS ko continuously pata rehna ki physical memory ka kaunsa portion kiske paas hai, kitna allocated hai, kitna free hai, aur kis process/segment ko diya gaya hai.
  - **Methods of Memory Tracking in Segmentation**
    1. Free List - Free list ek OS memory-tracking method hai jisme OS currently free physical memory blocks ki starting address aur size maintain karta hai, aur memory allocate karte waqt is list ko check karke suitable free block select karta hai; allocation ke baad us block ki information update/remove kar di jaati hai aur memory free hone par block ko list mein wapas add kiya jaata hai.
    2. Bitmap - Bitmap ek OS memory-tracking method hai jisme physical memory ko chhote fixed-size units mein divide karke har unit ke liye ek bit maintain ki jaati hai, jahan bit 0/1 batati hai ki unit free hai ya allocated, aur memory allocate karte waqt OS bitmap mein suitable free units dhoondhkar unhe allocated mark karta hai, jabki memory free hone par un bits ko wapas free mark kar deta hai.
    3. Boundary Tags/Block Metadata - Boundary Tags / Block Metadata ek memory-tracking method hai jisme har allocated ya free memory block ke saath us block ki size, status (free/allocated) aur zaroori information ka metadata store kiya jata hai, jisse OS/allocator block ko identify kar sake aur memory free karte waqt adjacent blocks ko check karke unhe merge (coalesce) karke bada free block bana sake.
   
# 4. Memory Allocation in Segmentation
Memory Allocation ka matlab hai OS/Memory Manager dwara physical memory mein kisi process ya uske segment ko uski required size ke according free memory ka suitable portion/block assign karna.

  - **Methods of Memory Allocation in Segmentation**
    1. First Fit - First Fit mein OS free memory blocks ko beginning se check karta hai aur jo pehla block required size ke barabar ya usse bada milta hai, usi mein process/segment ko allocate kar deta hai; poori memory mein best block dhoondhne ki zarurat nahi hoti.
    2. Best Fit - Best Fit mein OS available free memory blocks mein se sabse chhota aisa block select karta hai jo process/segment ki required size ke barabar ya usse bada ho, taaki allocation ke baad bacha hua unused space minimum rahe.
    3. Worst Fit - Worst Fit mein OS available free memory blocks mein se  sabse bada aisa block select karta hai jo process/segment ki required size ke barabar ya usse bada ho, aur usmein allocation karta hai; iske liye available blocks ko compare/scan karke largest suitable block choose kiya jata hai.

# 5. Memory Deallocation in Segmentation
Memory Deallocation ka matlab hai OS dwara process/segment ke use ke baad usko allocated physical memory block ko release karke wapas free memory mein available kar dena.

# 6. Address Translation
Address Translation ka matlab hai CPU se aaye virtual/logical address ko OS + hardware/MMU ki madad se corresponding physical RAM address mein convert karna, taaki actual memory location access ki ja sake.
  
  1. CPU virtual address generate karta hai, jisme segment aur us segment ke andar ka offset hota hai.
  2. Segment identify kiya jata hai, jaise Code, Heap ya Stack, taaki pata chale ki address kis segment se belong karta hai.
  3. Us segment ka Base address liya jata hai, jo physical RAM mein us segment ka starting address batata hai.
  4. Offset ko Bounds/Size se check kiya jata hai, taaki verify ho ki requested location segment ke valid range ke andar hai.
  5. Agar offset valid hai, to physical address calculate hota hai: Physical Address = Base + Offset.
  6. Agar offset Bounds se bahar hai, to access invalid maana jata hai aur hardware exception/segmentation fault generate karta hai.

# 7. Address Mappings
Address Mapping ka matlab hai virtual/logical address ko uske corresponding physical memory address se associate karna, yani OS/MMU ke paas ye information hona ki process ka kaunsa virtual address physical RAM ke kis address par located hai.
  1. Process ka virtual address space segments mein divided hota hai, jaise Code, Data, Heap aur Stack.
  2. Har segment ka ek separate mapping record hota hai, jisme us segment ka physical Base address aur Bounds/Size stored hota hai.
  3. Virtual address mein segment identifier + offset hota hai, jisse pata chalta hai ki access kis segment ke andar aur kis position par hai.
  4. Segment identifier ke basis par us segment ka Base address find kiya jata hai, aur offset ko us Base ke saath add kiya jata hai.
  5. Physical address = Segment Base + Offset hota hai.
  6. Bounds check bhi mapping ka part hai, jisme verify kiya jata hai ki offset us segment ki valid size ke andar hai; nahi to access reject ho jata hai.
