# 1. System Call 
System Call ek controlled interface (request mechanism) hai jiske through user-space program Kernel se kisi bhi Kernel service ko perform karne ki request karta hai.

# 2. System Call Wrapper
System call wrapper ek user-space library function hota hai jo application aur kernel ke beech interface ka kaam karta hai; yeh application dwara pass kiye gaye arguments ko operating system ke System Call ABI ke anusaar CPU registers me arrange karta hai, system call number set karta hai, syscall instruction execute karta hai, aur kernel se mila return value wapas application ko return karta hai.
