# 1. Operating System Interface
OS aur user/program ke beech interaction karne ka tarika. Yaani OS ke andar bahut saare complex kaam hote hain—files manage karna, processes chalana, memory manage karna, devices control karna etc. User ya application directly hardware se baat nahi karta; interface ke through OS ko request deta hai.

# 2. User <-> OS
User ka lia OS interface mainly 2 common forms mein hota hai. 
  1. GUI (Graphical User Interface) - Ishme graphics ka through user instructions deta hai like camera open karne ka lia camera icon click karna, file access ka lia folder par click krke filder par click karna etc.
  2. CLI (Command Line Interface) - Ishme user ksi Graphics ka through instructions nahi deta hai balki commands ka use karke instructions deta hai like linux ma ls, CD, mkdir etc.

# 3. Program <-> OS 
> Process ka lia OS mainly 3 types of interfaces provide karta hai.

1. SCI (System Call Interface) - System Call Interface woh controlled interface hai jiske through koi user-space program/process OS ke kernel se privileged services—jaise process create karna, file open/read/write karna, memory manage karna aur devices/network access karna—request karta hai, aur OS request ko check karke required operation perform karke result/error program ko return karta hai.
  - **Functions**
    1. SCI process/application ko OS kernel ki services request karne ka controlled interface provide karta hai.
    2. Process SCI ke through file, process, memory, networking aur device-related OS services request kar sakta hai.
    3. SCI user space aur kernel space ke beech controlled boundary/entry provide karta hai.
    4. SCI request ko kernel tak pahunchane ke liye system-call mechanism use karta hai, jahan kernel request ko validate karke service perform karta hai.
    5. SCI ka primary purpose process ko privileged kernel functionality tak controlled access dena hai.

  - **Internal Mechanism**
    1. System Call Invocation - User space program ko OS ki koi service cahiye hoti hai isiliye program system call interface ka through system call invoke karta hai. Example read().
    2. ABI ka according system call information prepare hoti hai - User space side system call ko invoke karne ka liya architecture ka ABI/calling convention ka according required information ko appropriate CPU registers mein place karti hai. Is information mein primarily system call number aur required arguments hote hai.
    3. System call entry instruction execute hota hai - User space execution ek special CPU instruction execute karti hai jo system call entry ka liye architecture dwara provide ki gyi hoti hai. Modern x86-64 linux mein commonly *syscall* instruction use hoti hai. Ya instruction normal function call jaisa simple user space function call nahi hai. Ishka purpose CPU ko kernel side system call entry mechanism mein transfer karna hai.
    4. CPU privilege transition karta hai - Special system call instruction ka result mein CPU user privilege level sa kernel privilege level mein controlled transition karta hai. x86-64 terminology mein normally CPL 3 -> CPL 0 transition hota hai. CPU predefined architecture rules ka according kernel execution context establish karta hai.
    5. Kernel ka syscall-entry point execute hota hai - CPU kernel ka predefined system call entry mechanism par control transfer karta hai. Ya kernel ka woh entry path hai jo system call requests receive karta hai.
    6. CPU/user execution state preserve ki jati hai - Kernel ko user space execution ko baad mein continue karna hota hai, isiliye required execution state ko preserve kiya jata hai. Is state mein relevant register values aur return ka liye required execution information hoti hai. Ishka purpose hai ki kernel ka kaam complete hone ka bad execution usi user space context mein correctly return kar sake.
    7. Kernel syscall-entry context establish karta hai - Kernel entry code ab kernel side execution ka liya required context establish karta hai. Is stage mein architecture specific entry handling aur kernel execution enviroment sa related low level setup hota hai. Kernel ensure karta hai ki execution ab proper kernel context mein hai aur system call path safely continue kar shakta hai.
    8. System call number identify hota hai - Kernel ko ab determine karna hota hai ki user na kaunsa system call request kiya hai. Ishke liye system call number use hota hai. Kernel system call number ko kernel ka defined syscall mapping ka according interpret karta hai.
    9. System call dispatch hota hai - Ab kernel ko identified system call ko uske corresponding kernel implementation tak route karna hota hai. Is routing ko system-call dispatch kehte hai. Kernel syscall number ka basis par appropriate syscall entry/handler select karta hai.
    10. Selected system call handler ko control milta hai - Handler ab requested OS service ko invoke karta hai.
    11. SCI boundary sa actual kernel service ko control transfer hota hai - Selected syscall handler request ko relevant kernel subsystem/service implementation tak forward karta hai. Yahin SCI aur actual OS service implementation ka beech boundary samajhna important hai. Ishke baad jo actual kaam hota hai jaisa file data access karna, memory allocate karna, process create karna, device operation karna wo SCI ka internal mechanism nahi hai.
**Return Process** 
    12. Kernel service ka result syscall handler ko return hota hai - Requested kernel service apna result syscall handler ko return karti hai. Result success ho shakta hai ya failure/error condition ho shakti hai.
    13. Syscall handler return value/error ko prepare karta hai - Syscall handler received result ko system-call return convention ka according prepare karta hai. Success mein appropriate return value hoti hai. Failure mein appropriate error indication/value hoti hai. Ya result user-space ko agreeb ABI/system call convention ka through return kiya jata hai.
    14. User Space execution state resotre ki jati hai - Kernel ab user space execution ko continue karna ka liye previously preserved execution state ko restore karta hai. Relevant registers/context ko appropriate state mein store restore kiya jata hai. Ishka purpose hai ki user program system call sa pehle jahan logically tha, ushke baad execution continue kar sake.
    15. Kernel sa user mode mein controlled return hota hai - Kernel ek architecture specific return mechanism use karke kernel privilege sa user privilege main return karta hai. x86-64 linux mein system call return ka liya sysret ya suitable return path use ho shakta hai, situation ka according.
    16. User space mein system call resutl receive hota hai - Ab execution user-space mein wapas aa chuki hoti hai. Finally application ko ab pta chal jata hai ki system call request successfull hui ya fail hui. 

      

2. API (Application Programming Interface) - API (Application Programming Interface) woh programming interface hai jiske through application/program apne code se kisi software component, library, service ya OS-provided functionality ko use karne ke liye predefined functions, methods aur rules ko call karta hai, aur woh API internally zarurat padne par System Calls ke through OS se service le sakti hai.
  - **Functions**
    1. API program ko kisi software component, library, framework ya service ki functionality use karne ka programmer-friendly interface provide karta hai.
    2. API predefined functions, methods, parameters aur return values ke through functionality access karne ka tarika define karta hai.
    3. API ka level generally SCI se higher hota hai, isliye programmer ko kernel-level details directly handle nahi karni padti.
    4. Agar API ki requested functionality ko kernel ki zarurat ho, to API ki implementation internally system call/SCI use kar sakti hai.
    5. API ka primary purpose programmer ko functionality ko easily aur consistently use karne dena hai, na ki directly kernel se communication karna.

3. ABI (Application Binary Interface) - ABI compiled binary code ke low-level interaction rules define karta hai—jaise arguments kaise pass honge, registers/stack kaise use honge, return value kaise milegi, aur binary components/system interfaces ke saath interaction ka exact convention kya hoga.
   - **Functions**
    1. ABI compiled binary code aur underlying libraries, OS/runtime ya other binary components ke beech interaction ke low-level rules define karta hai.
    2. ABI define karta hai ki function arguments kaise pass honge, return values kaise milengi aur registers/stack kaise use honge.
    3. ABI system-call interaction ke liye bhi architecture/platform-specific low-level conventions define kar sakta hai.
    4. ABI khud koi kernel service ya communication mechanism provide nahi karta, balki binary interaction ko compatible banane wale rules/contract provide karta hai.
    5. ABI ka primary purpose ye ensure karna hai ki compiled binary components ek agreed low-level convention ke according correctly interact kar saken.

  > API → functionality use karne ka interface
  > SCI → kernel service access karne ka interface
  > ABI → binary-level interaction ke rules/contract

# 4. OS Interface Design 
Operating System apni services ko users aur programs ke saamne kis tarah expose karega, unhe kaise access kiya jaayega, aur interaction ke rules kya honge — in sab ko design karna OS interface design hai.
