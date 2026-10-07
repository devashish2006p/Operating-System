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
