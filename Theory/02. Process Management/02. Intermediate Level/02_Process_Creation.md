# 1. ELF Executable Format  
ELF (Executable and Linkable Format) ek standard executable file format hai jo machine code, program metadata aur loading information ko ek fixed structure ma organize karta hai taki linux kernel usshe memory ma load karke execute kar sake. 

Jab compiler aur linker source code ko machine code ma convert karte hai, tab linker us machine code aur required metadata ko ELF format ma package karta hai aur bani hui file SSD/HDD par ek normal file ki tarah store ho jata hai. 

## 1.1 Internal Structure 
1. ELF Header - Ya ek bilkul beginning (offset 0) par maujood ek fixed size metadata structure hota hai jo Linux Kernel aur ELF tools ko btata hai ki ye file kis type ki hai, kis architecture ka lia bani hai, ishke ander baaki important structures kahan hai aur program execution kahan sa start hoga.
Ishka size ELF32 ma 52 bytes ka hota hai aur ELF64 ma 64 bytes ka hota hai.     
- **Internal Fields**
  1. e_ident - e_ident ELF Header ka pehla aur sabse important field hota hai, jo ELF file ki basic identity (pehchan) store karta hai. Isme aisi information hoti hai jisse kernel aur ELF tools turant pehchan lete hain ki ye file waqai ELF hai, kis architecture class (32/64-bit) ki hai, kis byte order (endianness) ka use karti hai, kis ABI ke liye bani hai, aur ELF format ka kaunsa version use kar rahi hai. Ya 16 bytes ka hota hai 32 aur 64 dono he types ka architecture ma.

  2. e_type - e_type ELF Header ka ek field hota hai jo batata hai ki ye ELF file kis type ki hai, yani system is file ko kis purpose ke liye use karega. Simple language ma ya ELF file ka category/type batane wala field hai. Ya dono he types ka architecture ma same 2 bytes ka hota hai. 

  3. e_machine - e_machine ELF Header ka field hota hai jo batata hai ki ye ELF file kis processor architecture (CPU architecture) ke liye banayi gayi hai, taaki operating system aur loader sahi processor ke hisaab se is file ko handle kar saken. Ya 2 bytes ka hota hai joki 32 aur 64 dono he architecture ma same size ka hota hai.

  4. e_version - e_version ELF Header ka field hota hai jo batata hai ki ELF file ELF format ke kis version ko follow karti hai, taaki loader aur tools us format ko sahi tarah interpret kar saken. Ya 4 bytes ka hota hai 32 aur 64 dono types ka architecture ma. 
  5. e_entry - ELF Header ka field hota hai jo program ke entry point (starting memory address) ko store karta hai, jahan se loader program ka execution shuru karwata hai. Ya 32 bits architecture ma 4 bytes ka hota hai aur 64 bits architecture ma 8 bytes ka hota hai. 
  6. e_phoff - e_phoff ELF Header ka field hota hai jo ELF file ke beginning se Program Header Table tak ka byte offset store karta hai, taaki loader us table ko locate kar sake. Ya 32 bits architecture ma 4 bytes ka hota hai aur 64 bits architecture ma 8 bytes ka hota hai. 
  7. e_shoff - e_shoff ELF Header ka field hota hai jo ELF file ke beginning se Section Header Table tak ka byte offset store karta hai, taaki tools us table ko locate kar saken. Ya 32 bits architecture ma 4 bytes ka hota hai aur 64 bits architecture ma 8 bytes ka hota hai. 
  8. e_flags - e_flags ELF Header ka field hota hai jo processor architecture ya operating system se related additional flags aur special configuration information store karta hai. Ya dono he architecutre ma same 4 bytes ka he hota hai.
  9. e_ehsize - e_ehsize ELF Header ka field hota hai jo poore ELF Header ka total size bytes me store karta hai, taaki loader ko header ki exact length pata chal sake. Ya dono he types of architecture ma 2 bytes ka hota hai. 
  10. e_phentsize - e_phentsize ELF Header ka field hota hai jo Program Header Table ki ek entry ka size bytes me store karta hai, taaki loader har entry ko sahi tarah read kar sake. Ya dono he types of architecture ma same 2 bytes ka hota hai. 
  11. e_phnum - e_phnum ELF Header ka field hota hai jo Program Header Table me total kitni entries hain, uski sankhya store karta hai. Ya 2 bytes ka hota hai dono he types of architectures ma. 
  12. e_shentsize - e_shentsize ELF Header ka field hota hai jo Section Header Table ki ek entry ka size bytes me store karta hai, taaki tools har section header ko sahi tarah read kar saken. Ya 2 bytes ka hota hai 32 aur 64 dono types of architecture ma. 
  13. e_shnum - e_shnum ELF Header ka field hota hai jo Section Header Table me total kitni section entries hain, uski sankhya store karta hai. Ya 2 bytes ka hota hai 32 aur 64 dono types ka architecture ma. 
  14. e_shstrndx - e_shstrndx ELF Header ka field hota hai jo Section Header Table me us entry ka index store karta hai jo section names wali string table (.shstrtab) ko represent karti hai, taaki tools har section ka naam dhoondh saken. Ya 2 bytes ka hota hai dono he types of architecture ma. 

2. Program Header Table - Ya ELF file ka ek metadata table hota hai jo operating system ka loader (Kernel) ko btata hai ki ELF file ka kaun-kaun sa parts ko memory ma load karna hai, kahan load karna hai, kitna load karna hai aur kis permission ka sath load karna hai. Simple language me Program Header Table = Kernel ka lia loading instructions ka table. Ishka koi fixed size nhi hota hai balki e_phentsize * e_phnum karke ishka actual size nikala jata hai. 
  - **Types of Entries in PHT**
  1. PT_NULL - Ya ek special entry type hai jo ksi bhi segment ko represent nahi karti aur loader ise ignore kar deta hai, ishka use unused ya placeholder entry ka roop ma hota hai. 

  2. PT_LOAD - PT_LOAD Program Header Table ki sabse important entry type hai, jo kernel ko batati hai ki ELF file ka kaunsa segment file se uthakar RAM me map/load karna hai, kitna load karna hai, kahan load karna hai, aur kis permission (Read/Write/Execute) ke saath load karna hai.
 
  3. PT_DYNAMIC - PT_DYNAMIC Program Header Table ki ek entry type hai jo kernel aur especially dynamic linker (ld-linux) ko batati hai ki ELF file ka .dynamic segment memory me kahan hai, taaki wahan se shared libraries, relocation, symbol table aur dynamic linking se judi baaki sari information padhkar program ko sahi tarah run kiya ja sake.
  
  4. PT_INTERP - PT_INTERP Program Header Table ki ek entry type hai jo kernel ko batati hai ki kis dynamically linked ELF program ko run karne ke liye kaunsa dynamic linker (interpreter), jaise /lib64/ld-linux-x86-64.so.2, pehle load aur execute karna hai, taaki wahi aage shared libraries load karke original program start kar sake.
  5. PT_NOTE - PT_NOTE Program Header Table ki ek entry hai jo kernel ya tools ko batati hai ki executable ki extra information (metadata), jaise Build ID aur ABI details, file me kahan rakhi hui hai.
  6. PT_PHDR - Program Header Table ki ek entry type hai jo kernel ya loader ko batati hai ki Program Header Table khud memory me kahan map hua hai, taaki zarurat padne par program ya dynamic linker usi Program Header Table ko runtime me access kar sake.
  7. PT_TLS - (Thread Local Storage) Program Header Table ki ek entry type hai jo loader ko batati hai ki thread-specific data (TLS) memory me kahan aur kitna allocate karna hai, taaki har thread ko us data ki apni alag private copy mile.
  8. PT_GNU_EH_FRAME - PT_GNU_EH_FRAME Program Header Table ki ek GNU-specific entry type hai jo loader aur runtime libraries ko batati hai ki exception handling aur stack unwinding (.eh_frame_hdr) ki information memory me kahan hai, taaki crash, exception ya function return ke samay call stack ko sahi tarah trace kiya ja sake.
  9. PT_GNU_STACK - PT_GNU_STACK Program Header Table ki ek GNU-specific entry type hai jo kernel ko batati hai ki process ki stack memory ko kis permission (Read, Write, aur Execute ya Non-Execute) ke saath create/map karna hai.
  10. PT_GNU_RELRO - PT_GNU_RELRO Program Header Table ki ek GNU-specific entry type hai jo kernel aur dynamic linker ko batati hai ki program ki kuch sensitive memory (jaise .got ke kuch parts) initialization ke baad read-only kar deni hai, taaki unhe runtime me modify karke attack na kiya ja sake.
  11. PT_GNU_PROPERTY - PT_GNU_PROPERTY Program Header Table ki ek GNU-specific entry type hai jo kernel aur loader ko batati hai ki is executable ke liye processor aur security se judi special properties (features) ki information memory me kahan rakhi hai, taaki program ko unke hisaab se safely aur correctly run kiya ja sake.

  - **Fields in each entries**
  1. p_type - p_type Program Header Entry ka sabse important field hai, jo loader ko batata hai ki ye entry kis type ki hai (jaise PT_LOAD, PT_DYNAMIC, PT_INTERP, PT_TLS), taaki uske hisaab se us entry ko process kiya ja sake.
  2. p_flags - p_flags Program Header Entry ka field hai jo loader/kernel ko batata hai ki is segment ko RAM me kis permission ke saath map karna hai—Read (R), Write (W), Execute (X) ya inka combination.
  3. p_offset - p_offset Program Header Entry ka field hai jo loader ko batata hai ki ELF file ke andar is segment ka data kis byte offset (kitne bytes aage) se shuru hota hai, taaki wahi se data uthakar RAM me map/load kiya ja sake.
  4. p_vaddr - p_vaddr Program Header Entry ka field hai jo loader/kernel ko batata hai ki is segment ko process ki virtual memory (RAM ke virtual address space) me kis virtual address par map/load karna hai.
  5. p_paddr - Program Header Entry ka field hai jo segment ka physical memory address store karta hai, lekin modern Linux user-space programs me ise kernel ignore karta hai aur iska practical use lagbhag nahi hota.
  6. p_filesz - p_filesz Program Header Entry ka field hai jo loader ko batata hai ki is segment ke liye ELF file se kitne bytes ka data read karke RAM me load/map karna hai.
  7. p_memsz - p_memsz loader/kernel ko batata hai ki is particular segment ke liye RAM me total kitni memory allocate/map karni hai, chahe usme se kuch data file me maujood ho ya na ho.
  8. p_align - p_align Program Header Entry ka field hai jo loader/kernel ko batata hai ki is segment ko RAM me kis alignment boundary (jaise 4 KB page boundary) par map/load karna hai, taaki memory access sahi aur efficient rahe.


3. Section - Section ELF file ka logical part hota hai jo compiler aur linker dwara ek hi type ke data (jaise code, initialized data, symbols, strings, relocation information, etc.) ko alag-alag organize karke store karne ke liye banaya jata hai.
- **Internal Sections** 
  - Code Sections : Ya ELF file ka ek section hai jishke kaam executable machine instructions ko organize aur store karna hota hai, aur ya machine instructions aur execution support code ko store karta hai. Ishko compiler banata hai aur linker final executable ma arrange karta hai aur loader/kernel memory ma map karta hai aur CPU in instructions ko execute karta hai. 
    1. .text - Ya ELF ka primary executable code section hota hai jiska kaam executable machine instructions ko organize aur store karna hota hai aur ya program ka actual machine code (functions ki instructions) ko store karta hai. Ishko compiler banata hai aur linker final executable me combine karta hai. Ishka use loader/kernel memory ma map karne ka lia karta hai aur CPU isi section ki ko use karta hai instructions execute karne ka lia. 
    2. .init - .init ELF file ka ek special code section hota hai jise compiler aur linker program ke main execution (main()) shuru hone se pehle chalne wali initialization instructions ko store karne ke liye banate hain. Is section ka purpose program ke runtime environment ko prepare karna hota hai, jaise runtime libraries ya initialization routines ko execute karna, taaki program execute hone se pehle sab zaruri setup complete ho jaye. Isme executable machine instructions store hote hain jo initialization ka kaam karte hain, na ki normal program logic. Program load hone ke baad loader aur dynamic linker runtime environment ko initialize karte waqt is section ki instructions ko execute karwate hain, aur uske baad control main() ya entry point ke normal execution flow ko de diya jata hai.
    3. .fini - .fini ELF file ka ek special code section hota hai jise compiler aur linker un machine instructions ko store karne ke liye banate hain jo program ke apna kaam complete karne aur exit hone ke baad chalni hoti hain. Is section ka main purpose program ke shutdown ya cleanup process ko perform karna hota hai, taaki runtime environment ko properly close kiya ja sake aur zaruri finalization routines execute ho saken. Isme executable machine instructions store hoti hain jo cleanup aur finalization se related hoti hain, na ki program ka normal business logic. Jab program terminate hone lagta hai, tab runtime system aur dynamic linker in finalization routines ko execute karwate hain, aur CPU in instructions ko chala kar program ka shutdown process complete karta hai.
    4. .plt - Ya ELF ka ek special code section hai jo external (shared library) functions ko call karta hai jab unka address pehle sa known na ho. Ya Machine code stubs (choti instructions) ko store karta hai jo dynamic linker tak control pahunchati hai. Ya linker banata hai aur ishka use CPU aur indirectly dynamic linker karta hai. 
 
  - Read-only Data : Read-Only Data Sections ELF file ke wo sections hote hain jinhe compiler un sabhi data ko alag aur surakshit tarike se organize karke store karne ke liye banata hai jo program ke chalne ke dauran sirf padha (read) jata hai aur badla (write) nahi jana chahiye, linker in sections ko final executable me sahi jagah arrange karta hai, loader/kernel inhe memory me read-only permission ke saath map karta hai taaki koi program ya attacker in data ko runtime me modify na kar sake, aur program ka code, runtime libraries ya debugger zarurat padne par isi data ko padhkar program ko sahi tarike se chalate hain.
    1. .rodata
    2. .eh_frame
    3. .eh_frame_hdr
  - Writable Data - Writable Data Sections ELF file ke wo sections hote hain jinhe compiler un sabhi data ko alag aur vyavasthit tarike se organize karke store karne ke liye banata hai jinka value program ke chalne ke dauran badal sakta hai ya badalna zaruri hota hai, jaise initialized variables, uninitialized variables aur runtime me update hone wala writable data, linker in sections ko final executable ya shared library me sahi jagah arrange karta hai, loader/kernel inhe RAM me read aur write permissions ke saath map karta hai taaki program in data ko runtime me padh aur badal sake, aur program ka code, runtime libraries aur kabhi-kabhi dynamic linker bhi in sections ka upyog program ki state, variables aur runtime information ko maintain aur update karne ke liye karte hain.
    .data
    .bss
    .got
    .got.plt
    .init_array
    .fini_array
  - Dynamic Linking - Dynamic Linking Sections ELF file ke wo sections hote hain jinhe compiler aur linker milkar isliye banate hain taaki program ko chalane ke liye zaruri shared libraries, unke symbols, function aur variable names, library dependencies aur dynamic linking se sambandhit metadata ko alag aur sangathit roop me store kiya ja sake; linker in sections me required dynamic linking information bhar kar final executable ya shared library banata hai, aur program ke load hone ya runtime ke dauran dynamic linker (ld-linux) inhi sections ko padhkar required shared libraries ko load karta hai, symbols ko resolve karta hai aur program ko un external libraries ke saath sahi tarike se jodkar chalata hai.
    .dynamic
    .dynsym
    .dynstr
    .gnu.hash
    .hash
    .gnu.version
    .gnu.version_r
  - Relocation : Ya ELF file ka ek section hai joki relocation information ko rakhta hai ishka kaam hai un addresses ko store karke rakhna jinko baad ma fix/update karna hai, ya relocation entries (kin locations ka addresses badalne hai aur unse judi metadata) ko store karta hai. Compiler object files ka lia relocation information generate karta hai aur linker use final ELF ma required relocation sections ka roop ma arrange karta hai. Ishka use linker build time par karta hai aur dynamic linker runtime par karta hai. 
    .rela.dyn
    .rela.plt
  - Symbol & String Tables - Symbol & String Table Sections ELF file ke wo sections hote hain jinhe compiler aur linker program me maujood functions, variables, labels aur anya symbols ki pahchan (identity) aur unke naam ko sangathit roop me store karne ke liye banate hain, jisme symbol tables pratyek symbol se sambandhit jankari (jaise uska naam kis string se juda hai, uska address ya value, size aur anya attributes) rakhti hain aur string tables un symbols ke actual text names ko store karti hain; linker in sections ka upyog symbols ko identify aur resolve karne ke liye karta hai, dynamic linker runtime par external symbols ko resolve karne ke liye inka upyog karta hai, aur debugger, disassembler tatha anya binary analysis tools bhi inhi sections ki madad se binary ke machine addresses ko human-readable function aur variable names se jodkar program ko samajhte aur analyze karte hain.
    .symtab
    .strtab
    .shstrtab
  - Notes : Note Sections ELF file ke wo sections hote hain jinhe compiler, linker ya build tools program se sambandhit atirikt (supplementary) jankari ko sangathit roop me store karne ke liye banate hain, jisme program ke baare me aisi metadata rakhi jati hai jo program ke machine code ka hissa nahi hoti, jaise build identification, operating system ya ABI se sambandhit jankari, processor ya security properties aur anya descriptive information; loader, kernel, dynamic linker ya binary analysis tools zarurat padne par in note sections ko padhkar program ki compatibility, security features aur build se judi jankari prapt karte hain, lekin program ke normal instructions ko execute karne ke liye in sections ka seedha upyog nahi kiya jata.
    .note.gnu.property
    .note.gnu.build-id
    .note.ABI-tag

4. Section Header Table

 
---
## 1.2 Internal Mechnaism of ELF File Creation
### Phase 1: Source Code Exists
1. Programmer source code file create karta hai (jaise hello.c).
2. Source code SSD/HDD par normal text file ke roop me store hota hai.
3. Is stage par file executable nahi hoti.
4. Is stage par file ELF bhi nahi hoti.
5. User compiler ko source code compile karne ka command deta hai.
### Phase 2: Compiler Invocation
6. Shell gcc command ko parse karti hai.
7. Shell gcc executable ko run karne ke liye kernel ko request bhejti hai.
8. Kernel gcc process create karta hai.
9. Scheduler gcc ko CPU allocate karta hai.
10. GCC execution start karta hai.
### Phase 3: Preprocessing
11. GCC preprocessor phase start karta hai.
12. #include directives identify kiye jate hain.
13. Required header files locate ki jati hain.
14. Header file contents source code me insert kiye jate hain.
15. Macros expand kiye jate hain.
16. Conditional compilation directives evaluate kiye jate hain.
17. Preprocessed source file generate hoti hai.
### Phase 4: Compilation
18. Compiler preprocessed source code read karta hai.
19. Source code ko lexical tokens me break kiya jata hai.
20. Syntax analysis perform kiya jata hai.
21. Parse tree construct kiya jata hai.
22. Semantic checks perform kiye jate hain.
23. Variables aur functions validate kiye jate hain.
24. Compiler internal representation generate karta hai.
25. Optimization passes apply ho sakte hain.
26. Compiler machine instructions generate karta hai.
27. Assembly code output create hota hai.
### Phase 5: Assembly
28. Assembler assembly code read karta hai.
29. Assembly mnemonics ko machine instructions me convert kiya jata hai.
30. Machine code bytes generate hote hain.
31. Initial sections create kiye jate hain (.text, .data, .bss, etc.).
32. Symbol table generate ki jati hai.
33. Relocation entries create ki jati hain.
34. ELF object file (.o) generate hoti hai.
35. Ye object file SSD/HDD par store hoti hai.
### Phase 6: Linker Start
36. Linker object file read karta hai.
37. Linker required libraries identify karta hai.
38. Startup runtime code identify kiya jata hai.
39. Additional object files collect kiye jate hain.
40. Linking process start hoti hai.
### Phase 7: Section Collection
41. Linker sab object files ke sections inspect karta hai.
42. Sab .text sections collect kiye jate hain.
43. Sab .data sections collect kiye jate hain.
44. Sab .rodata sections collect kiye jate hain.
45. Sab .bss sections collect kiye jate hain.
46. Similar type ke sections merge kiye jate hain.
47. Final executable layout prepare hona start hota hai.
### Phase 8: Symbol Resolution
48. Linker unresolved symbols identify karta hai.
49. Function references inspect kiye jate hain.
50. Variable references inspect kiye jate hain.
51. Required symbol definitions locate ki jati hain.
52. Library symbol tables search ki jati hain.
53. Matching symbols find kiye jate hain.
54. Symbol references actual definitions se connect kiye jate hain.
55. Unresolved references resolve kiye jate hain.
### Phase 9: Relocation
56. Linker memory layout calculate karta hai.
57. Har section ka final address calculate hota hai.
58. Placeholder addresses identify kiye jate hain.
59. Function call targets calculate kiye jate hain.
60. Variable addresses calculate kiye jate hain.
61. Relocation records process kiye jate hain.
62. Machine code ke andar actual addresses patch kiye jate hain.
63. Final executable addresses insert kiye jate hain.
### Phase 10: ELF Structure Construction
64. Linker final ELF file create karna start karta hai.
65. ELF Header structure create ki jati hai.
66. Program Header Table create ki jati hai.
67. Final sections place kiye jate hain.
68. Dynamic linking information create ki jati hai.
69. Symbol information arrange ki jati hai.
70. Section metadata arrange ki jati hai.
### Phase 11: ELF Header Creation
71. ELF magic number set kiya jata hai.
72. Architecture information set ki jati hai.
73. File type set kiya jata hai.
74. Entry point address calculate kiya jata hai.
75. Program Header location set ki jati hai.
76. Section Header location set ki jati hai.
77. ELF Header finalize kiya jata hai.
### Phase 12: Program Header Creation
78. Loadable segments identify kiye jate hain.
79. Har segment ke permissions define kiye jate hain.
80. Read permissions set ki jati hain.
81. Write permissions set ki jati hain.
82. Execute permissions set ki jati hain.
83. Virtual memory mapping information generate ki jati hai.
84. Program Header entries finalize ki jati hain.
### Phase 13: Entry Point Setup
85. Linker process startup address determine karta hai.
86. Usually _start function identify kiya jata hai.
87. Entry point field update ki jati hai.
88. Kernel ko future execution start location provide ki jati hai.
### Phase 14: Final File Generation
89. ELF Header file me write kiya jata hai.
90. Program Headers file me write kiye jate hain.
91. Sections file me write kiye jate hain.
92. Dynamic information write ki jati hai.
93. Symbol information write ki jati hai.
94. Section Header Table write ki jati hai.
95. Final ELF byte stream generate hoti hai.
96. ELF file SSD/HDD par save kar di jati hai.
### Phase 15: ELF Ready
97. Ab executable ELF file complete ho chuki hoti hai.
98. Abhi bhi koi process create nahi hua hota.
99. File disk par passive form me store hoti hai.
100. Jab user executable run karega tab kernel ELF ko read karke process creation flow start karega.

## 1.3 ELF Loading Mechanism
1. Kernel executable file open karta hai.
2. Kernel ELF file ke first bytes read karta hai.
3. ELF magic number verify karta hai.
4. ELF class verify karta hai (32-bit ya 64-bit).
5. Architecture verify karta hai (x86, ARM, etc.).
6. ELF type verify karta hai (Executable ya Shared Object).
7. ELF Header complete read kiya jata hai.
8. ELF Header se Program Header Table ka offset nikala jata hai.
9. Kernel Program Header Table locate karta hai.
10. Program Header entries read ki jati hain.
11. Har Program Header inspect kiya jata hai.
12. Kernel identify karta hai kaunsa header loadable segment represent karta hai.
13. Har loadable segment ka file offset read kiya jata hai.
14. Har loadable segment ka virtual address read kiya jata hai.
15. Har loadable segment ka file size read kiya jata hai.
16. Har loadable segment ka memory size read kiya jata hai.
17. Har loadable segment ki permissions read ki jati hain (R/W/X).
18. Kernel segment mapping information prepare karta hai.
19. Text segment identify hota hai.
20. Read-only data segment identify hota hai.
21. Data segment identify hota hai.
22. BSS region identify hoti hai.
23. Entry point address ELF Header se read kiya jata hai.
24. Dynamic linking information locate ki jati hai (agar present ho).
25. Interpreter path locate kiya jata hai (agar dynamic executable ho).
26. ELF file ki loading metadata complete ho jati hai.
27. Kernel ke paas ab complete information hoti hai ki:
  - kya load karna hai,
  - kahan map karna hai,
  - kis permission ke saath map karna hai,
  - execution kahan se start karna hai.

## 1.4 Linux Tools for ELF
1. file - file Linux ka ek file-identification tool hai jo kisi file ke naam ya extension par bharosa karne ke bajay uske actual content (bytes, magic number aur metadata) ko inspect karke batata hai ki woh file asal me kis type ki hai, jaise ELF executable, text file, image, PDF, archive ya koi aur format.
- **Major Flags**
  - 1. file <binary> : Ye command file ko analyze karke batata hai ki woh ELF executable hai, script hai, image hai ya koi aur format, basically initial identification karta hai.
    2. file -b <binary> : Ye same analysis karta hai but file name hide karke sirf pure result deta hai, jo scripting ya clean output ke liye use hota hai.
    3. file -i <binary> : Ye file ka MIME type show karta hai, jaise application/x-executable, jo quick categorization me help karta hai ki file executable hai ya data file.
    4. file -L <binary> : Agar file ek symbolic link hai, to ye actual original file ko follow karke uska type check karta hai, na ki sirf shortcut ko.
    5. file -s <binary> : Ye special files (jaise device files ya raw disk data) ko bhi read karke unke andar ka actual content identify karta hai, jo forensic analysis me useful hota hai.
    6. file -z <binary> : Ye compressed files ke andar bhi jaake check karta hai ki andar koi ELF binary ya executable hai ya nahi, useful for packed malware or zip analysis.

- **When is it use?**
  1. Jab tumhe koi unknown file milti hai aur tumhe pata karna hota hai ki woh kya hai (ELF, text, image, etc.), tab file use hota hai.
  2. Jab tum suspicious file (malware ya unknown binary) analyze kar rahe hote ho, tab file pehle run karke uski identity check karte ho.
  3. Jab reverse engineering start karne se pehle confirm karna hota hai ki file ELF executable hai ya nahi, tab file use hota hai.
  4. Jab tumhe binary ka architecture (x86, x64, ARM) aur type (statically/dynamically linked) jaldi se dekhna hota hai, tab file use hota hai.
  5. Jab symbolic link (shortcut file) ka actual target check karna hota hai, tab file -L use hota hai.
  6. Jab multiple files ko quickly check karke unka type identify karna hota hai (automation/scripts), tab file -b use hota hai.
  7. Jab compressed ya packed files ke andar hidden executable check karna hota hai, tab file -z use hota hai.
  8. Jab raw device ya disk file ka content identify karna hota hai (forensics), tab file -s use hota hai.

2. readelf - readelf ek Linux tool hai jo ELF file ke andar ka complete structure (headers, sections, entry point, memory layout) ko readable form me dikhata hai bina file ko run kiye.
- **Major Flags**
  1. readelf -h : Ye ELF file ka main header dikhata hai, jisme pata chalta hai file 32/64-bit hai, entry point kya hai, aur kis type ka executable hai.
  
  2. readelf -S : Ye ELF file ke sections (like .text, .data, .bss) dikhata hai, jisse pata chalta hai program ka code aur data ka structure kaise divided hai.
  
  3. readelf -l : Ye batata hai ki program memory me kaise load hoga (segments) aur OS usko kaise execute karega.
  
  4. readelf -s : Ye file ke andar ke functions aur variables (symbols) dikhata hai, jaise main, printf, etc.
  
  5. readelf -d : Ye batata hai ki binary kaun-kaun si shared libraries (.so files) use kar raha hai, yani dependencies kya hain.
  
  6. readelf -r : Ye show karta hai ki binary me relocation entries kaise handle ho rahi hain, matlab dynamic linking ka internal adjustment.
  
  7. readelf -A : Ye architecture-specific details dikhata hai (CPU type, ABI info), jo batata hai binary kis system ke liye optimized hai.
  
  8. readelf -a : Ye sab important information ek saath dump karta hai (header + sections + symbols + etc.), quick full overview ke liye.

- **When is it use?**
  1. Jab tumhe kisi ELF binary ka basic structure (header info) dekhna hota hai, tab readelf -h use hota hai.
  2. Jab tumhe program ke sections (.text, .data, .bss) samajhne hote hain ki code aur data kaise divided hai, tab readelf -S use hota hai.
  3. Jab tumhe samajhna hota hai ki binary memory me kaise load hoga (execution mapping), tab readelf -l use hota hai.
  4. Jab tumhe binary ke andar ke functions aur variables (symbols) dekhne hote hain, tab readelf -s use hota hai.
  5. Jab tumhe pata karna hota hai ki program kaun-kaun si shared libraries use kar raha hai, tab readelf -d use hota hai.
  6. Jab tumhe dynamic linking ya runtime adjustments (relocation entries) dekhni hoti hain, tab readelf -r use hota hai.
  7. Jab tumhe CPU architecture ya system-specific details samajhni hoti hain, tab readelf -A use hota hai.
  8. Jab tumhe ek baar me poori ELF file ka overview (full analysis) chahiye hota hai, tab readelf -a use hota hai.

3. objdump - objdump ek Linux tool hai jo binary file (ELF) ko machine-level aur assembly level me break karke dikhata hai, taaki tum samajh sako ki program actually execute kaise ho raha hai.
- **Major Flags**
  1. objdump -d : Ye binary ko assembly (CPU instructions) me convert karke dikhata hai, jisse pata chalta hai program actually kaise execute ho raha hai.
  
  2. objdump -D : Ye poori file ke har section ka assembly dump karta hai, even non-executable parts bhi, deep analysis ke liye.
  
  3. objdump -S : Ye assembly ke saath source code mix karke dikhata hai, agar debugging symbols present ho.
  
  4. objdump -h : Ye binary ke sections ka overview deta hai (like .text, .data, .bss), structure samajhne ke liye.
  
  5. objdump -t : Ye binary ke symbols (functions, variables) dikhata hai, jaise main, printf, etc.
  
  6. objdump -x : Ye full information dump karta hai (headers + sections + symbols ek saath).
  
  7. objdump -r : Ye relocation entries dikhata hai, matlab dynamic linking ka internal adjustment kaise ho raha hai.
  
  8. objdump -s : Ye binary ka raw hex + ASCII content dikhata hai (memory-level view).
- **When is it use?**
  1. Jab tumhe kisi binary ka assembly code dekhna hota hai, tab objdump -d use hota hai.
  2. Jab tumhe samajhna hota hai ki program CPU instructions me kaise execute ho raha hai, tab objdump use hota hai.
  3. Jab tum reverse engineering kar rahe hote ho aur binary ka logic samajhna hota hai bina source code ke, tab objdump use hota hai.
  4. Jab tumhe functions (jaise main, printf) ka low-level behavior dekhna hota hai, tab objdump -t use hota hai.
  5. Jab tumhe binary ka full structure + headers + symbols ek saath dekhna hota hai, tab objdump -x use hota hai.
  6. Jab tumhe program ka memory content (hex + ASCII form) analyze karna hota hai, tab objdump -s use hota hai.
  7. Jab tumhe samajhna hota hai ki binary dynamic linking ya relocation kaise handle karta hai, tab objdump -r use hota hai.
  8. Jab tumhe debugging symbols ke saath source code + assembly mixed view chahiye hota hai, tab objdump -S use hota hai.

4. nm - nm ek Linux tool hai jo ELF binary (ya object file) ko read karke uske andar ke saare symbols jaise functions, global variables, aur external undefined references ke names aur addresses ko list karta hai, taaki tum bina code run kiye samajh sako ki program ke andar kya-kya “named components” exist karte hain aur kaunse functions internally defined hain ya external libraries se link hone wale hain.
- **Major Flags**
1. nm file : Binary ke saare symbols (functions + variables) dikhata hai.

2. nm -g file : Sirf global symbols dikhata hai (jo externally visible hote hain).

3. nm -u file : Sirf undefined symbols dikhata hai (jo external libraries se aayenge).

4. nm -C file : C++ names ko readable form me demangle karta hai (mangled names ko normal banata hai).

5. nm -n file : Symbols ko address order (memory order) me sort karke dikhata hai.

6. nm -a file : saare symbols (even hidden/debug) bhi show karta hai.
- **When is it use?**
  1. Jab tumhe kisi binary ke andar ke functions ke names (jaise main, custom functions) dekhne hote hain, tab nm use hota hai.
  2. Jab tumhe samajhna hota hai ki program me kaun-kaun se global variables define hain, tab nm use hota hai.
  3. Jab tum reverse engineering kar rahe hote ho aur binary ke hidden symbol names identify karne hote hain, tab nm use hota hai.
  4. Jab tumhe pata karna hota hai ki program ke andar kaunse functions external libraries se link honge (undefined symbols), tab nm -u use hota hai.
  5. Jab tum debugging ya vulnerability analysis kar rahe hote ho aur function mapping samajhni hoti hai, tab nm use hota hai.
  6. Jab tumhe C++ binaries ke mangled function names ko readable form me dekhna hota hai, tab nm -C use hota hai.
  7. Jab tumhe binary ke symbols ko memory address order me analyze karna hota hai, tab nm -n use hota hai.

5. strings - strings ek Linux tool hai jo binary file ko scan karke uske andar se saara human-readable text (jaise messages, passwords, URLs, error strings, function names aur hidden hints) extract karke show karta hai, taaki tum bina program run kiye samajh sako ki us binary ke andar kya information chhupi hui hai.

- **Major Flags**
1. strings file : Binary ke andar se saare readable text (ASCII strings) extract karta hai, jaise messages, functions, URLs, etc.

2. strings -n <number> : Sirf minimum length wale strings show karta hai (noise kam karne ke liye), jaise strings -n 6 file.

3. strings -e l file : Binary ke andar se Unicode / 16-bit strings bhi extract karta hai, jo normal strings me nahi dikhti.

4. strings -t x file : Har string ke saath uska hex memory offset bhi show karta hai, jisse pata chalta hai string file ke kis location par hai.
- **When is it use?**
1. Jab tumhe kisi binary ke andar se hidden readable text (messages, passwords, URLs) nikalne hote hain, tab strings use hota hai.
2. Jab tum reverse engineering kar rahe hote ho aur bina run kiye clues collect karne hote hain, tab strings use hota hai.
3. Jab tum malware analysis kar rahe hote ho aur suspicious words jaise “http”, “/bin/bash”, “cmd” dhoondhne hote hain, tab strings use hota hai.
4. Jab binary stripped hoti hai (symbols remove hote hain) aur nm useful nahi hota, tab strings use hota hai.
5. Jab tumhe samajhna hota hai ki program kaunse libraries, functions ya dependencies use kar raha hai, tab strings use hota hai.
6. Jab tumhe kisi unknown file ka quick initial idea lena hota hai bina deep analysis ke, tab strings use hota hai.

6. size - size function ek Linux tool hai jo kisi binary ya object file ke **different sections (.text, .data, .bss) ka memory size batata hai aur total program kitna space occupy karega woh show karta hai.
- **Major Flags**
1. size file : Ye binary ke sections ka size show karta hai:
  - .text (code size)
  - .data (initialized data)
  - .bss (uninitialized data)
  - total size
2. size -A file: Ye ELF ke har section ka detailed breakdown + architecture info dikhata hai, deeper analysis ke liye.

3. size -B file : Ye output ko BSD-style format me show karta hai (format change, same data).
- **When is it use?**
  1. Jab tumhe kisi ELF binary ka memory footprint (kitna space use hua hai) dekhna hota hai, tab size use hota hai.
  2. Jab tum program build karte ho aur check karna hota hai ki code (.text) aur data (.data/.bss) kitna memory le raha hai, tab size use hota hai.
  3. Jab tum embedded systems ya optimization kar rahe hote ho aur binary ko lightweight banana hota hai, tab size use hota hai.
  4. Jab tum compiler output analyze kar rahe hote ho aur dekhna hota hai ki changes ke baad binary size badha ya ghata, tab size use hota hai.
  5. Jab tum reverse engineering ke starting stage me ho aur quick idea lena hota hai program ka memory structure ka, tab size use hota hai.

7. ldd (List Dynamic Libraries Dependencies) - ldd ek Linux tool hai jo kisi ELF executable ko run kiye bina batata hai ki ye binary kaun-kaun si shared libraries (.so files) par depend karti hai aur runtime par un libraries ko system me kahan se load kiya jayega.
- **Major Flags**
  1. ldd file : Binary ki saari shared library dependencies aur unke locations dikhata hai.
  
  2. ldd -v file : Libraries ke saath version information bhi dikhata hai, jisse pata chalta hai binary ko kaunsi library versions chahiye.
  
  3. ldd -u file : Unused libraries dikhata hai, yani linked to hain lekin actually use nahi ho rahi.
  
  4. ldd -r file : Shared libraries ke functions aur relocations ko check karta hai aur missing symbols ki report deta hai.
  
  5. ldd -d file : Data relocations check karta hai aur runtime linking issues detect karta hai.
- **When is it use?**
  1. Jab program run nahi ho raha ho aur dependency issue check karna ho.
  2. Jab dekhna ho ki binary kaun-kaun si .so libraries use karti hai.
  3. Jab reverse engineering me binary ke external dependencies samajhne ho.
  4. Jab missing library ya missing symbol error troubleshoot karna ho.
  5. Jab dynamic linking ka behavior analyze karna ho.

8. hexdump - hexdump ek Linux tool hai jo kisi file (ELF, image, text, disk, memory dump, etc.) ke raw bytes ko hexadecimal aur ASCII format me dikhata hai, taaki tum file ke sabse lowest level data ko exactly waise dekh sako jaise woh storage ya memory me physically stored hota hai, bina kisi interpretation ya processing ke.
- **Major Flags**
  1. hexdump -C file : Sabse important flag; file ke bytes ko hex + ASCII side-by-side dikhata hai, jisse raw data aur readable text dono ek saath nazar aate hain.
  
  2. hexdump -n <number> file : Sirf pehle <number> bytes dikhata hai, jab tum file ka initial portion (jaise ELF magic bytes) dekhna chahte ho.
  
  3. hexdump -s <offset> : Kisi specific offset se reading start karta hai, jab tum file ke beech ke kisi region ko inspect karna chahte ho.
  
  4. hexdump -C -s <offset> -n <number> file : Kisi exact location se exact number of bytes dekhne ke liye (real-world analysis me bahut common combination).
  
  5. hexdump file : Default hex dump deta hai, lekin practical analysis me log zyada tar -C hi use karte hain.
- **When is it use?**
  1. Jab tum kisi file ke raw bytes ko exactly waise dekhna chahte ho jaise woh disk ya memory me stored hain, tab hexdump use hota hai.
  2. Jab tum verify karna chahte ho ki file sach me ELF file hai ya nahi, tab ELF magic bytes (7f 45 4c 46) dekhne ke liye hexdump use hota hai.
  3. Jab tum kisi file ke specific offset (particular byte location) par kya data rakha hai ye dekhna chahte ho, tab hexdump use hota hai.
  4. Jab tum reverse engineering kar rahe hote ho aur binary ke actual byte-level content ko inspect karna hota hai, tab hexdump use hota hai.
  5. Jab tum dekhna chahte ho ki koi string, number, address ya structure file ke andar physically kaise stored hai, tab hexdump use hota hai.
  6. Jab higher-level tools (file, readelf, strings) jo information de rahe hain usko raw bytes se verify karna hota hai, tab hexdump use hota hai.
  7. Jab tum file corruption, binary patching ya forensic analysis kar rahe hote ho aur exact bytes check karne hote hain, tab hexdump use hota hai.

9. xxd - xxd ek tool hai jo kisi file ke raw bytes ko hexadecimal format me dikhata hai aur zaroorat padne par hexadecimal data ko wapas original binary file me convert bhi kar sakta hai.
- **Major Flags**
  1. xxd file : File ke raw bytes ko hex + ASCII format me dikhata hai.
  
  2. xxd -l <bytes> file : Sirf shuru ke <bytes> bytes dikhata hai. ELF header ke starting bytes dekhne ke liye useful.
  
  3. xxd -s <offset> file
  File ke kisi specific offset se dump start karta hai.
  
  4. xxd -c <number> file : Ek line me kitne bytes dikhane hain, ye control karta hai.
  
  5. xxd -b file : Bytes ko hexadecimal ki jagah binary (0 aur 1) me dikhata hai.
  
  6. xxd -r : Hex dump ko wapas original binary file me convert karta hai.


- **When is it use?**
  Jab tum file ke raw bytes ko dekhna chahte ho.
  Jab ELF file ke starting bytes ya specific offset inspect karna ho.
  Jab tum binary patching kar rahe ho.
  Jab tum hex data ko wapas binary file me convert karna chahte ho.
  Jab tumhe hexdump jaisa output chahiye lekin reverse conversion ki capability bhi chahiye.

10. strace - strace ek Linux tool hai jo kisi program ke execution ke dauran kernel ke saath hone wali saari system calls (jaise open(), read(), write(), execve()) aur unke results ko monitor karke dikhata hai.
- **Major Flags**
  1. strace program : Ye program ke execution ke dauran kernel ko bheji gayi har system call aur uska return result dikhata hai.
  
  2. strace -o output.txt program : Ye program ki saari system calls ko terminal par dikhane ke bajay ek file me save karta hai taaki baad me aaram se analysis kiya ja sake.
  
  3. strace -e trace=open,read,write program : Ye sirf unhi system calls ko dikhata hai jo tum specify karte ho aur baaki sab system calls ko hide kar deta hai taaki output me unnecessary noise kam ho jaye.
  
  4. strace -f program : Ye main process ke saath-saath uske dwara create kiye gaye saare child processes aur threads ki system calls ko bhi trace karta hai.
  
  5. strace -p PID : Ye kisi already running process ke saath attach hokar uski current aur future system calls ko live monitor karta hai bina process restart kiye.
  
  6. strace -c program : Ye program ke khatam hone ke baad har system call kitni baar execute hui aur usme kitna time laga uska statistical summary report deta hai.
  
  7. strace -t program : Ye har system call ke saamne exact timestamp add karta hai taaki pata chale kis time par kaunsi system call execute hui thi.
  
  8. strace -e trace=file program : Ye sirf file-related system calls jaise file open karna, read karna, write karna aur close karna dikhata hai aur baaki categories ki system calls ko filter kar deta hai.
- **When is it use?**
  1. Jab tum dekhna chahte ho ki program execution ke dauran kernel ko kaun-kaun si system calls bhej raha hai, tab strace use hota hai.
  2. Jab tum samajhna chahte ho ki ELF file process banne ke baad kernel ke saath kaise interact kar rahi hai, tab strace use hota hai.
  3. Jab koi program error de raha ho aur tum pata lagana chahte ho ki kis system call par failure hua, tab strace use hota hai.
  4. Jab tum dekhna chahte ho ki program kaunsi files open, read, write aur close kar raha hai, tab strace use hota hai.
  5. Jab tum dekhna chahte ho ki program memory allocate karne ke liye kernel se kya requests kar raha hai, tab strace use hota hai.
  6. Jab tum dekhna chahte ho ki program network connections kab aur kaise establish kar raha hai, tab strace use hota hai.
  7. Jab tum dekhna chahte ho ki program naye processes ya threads create kar raha hai ya nahi, tab strace use hota hai.
  8. Jab tum malware ya unknown binary ka behavior analyze karna chahte ho aur dekhna chahte ho ki woh system ke saath kya-kya actions perform kar rahi hai, tab strace use hota hai.
  9. Jab tum OS aur Linux process execution ko practical level par samajhna chahte ho, tab strace use hota hai.
  10. Jab tum kisi running process ki live activity monitor karna chahte ho bina usse restart kiye, tab strace use hota hai.

11. ltrace - ltrace ek Linux tool hai jo kisi program ke execution ke dauran shared libraries (jaise libc.so) ke functions calls (jaise printf(), malloc(), puts()) aur unke arguments/return values ko monitor karke dikhata hai.
- **Major Flags**
  - Wahi flags joki strace ma use hota hai kafi had tak ishme v use hote hai.
- **When is it use?**
1. Jab tum dekhna chahte ho ki program execution ke dauran kaun-kaun se library functions (jaise printf(), malloc(), strlen()) call kar raha hai, tab ltrace use hota hai.
2. Jab tum reverse engineering kar rahe hote ho aur binary ke internal behavior ko library function calls ke through samajhna chahte ho, tab ltrace use hota hai.
3. Jab tum dekhna chahte ho ki program memory allocation, string handling ya input/output ke liye kaunse library functions use kar raha hai, tab ltrace use hota hai.
4. Jab tum kisi unknown ya suspicious binary ka analysis kar rahe hote ho aur uske library-level actions ko observe karna chahte ho, tab ltrace use hota hai.
5. Jab tum program → library → kernel ke execution flow ko samajhna chahte ho aur dekhna chahte ho ki system call hone se pehle kaunsa library function call hua tha, tab ltrace use hota hai.

12. gdb - GDB (GNU Debugger) ek debugging tool hai jo kisi running ya paused program ko step-by-step execute karne, variables ki values dekhne, memory inspect karne, registers analyze karne aur program crash ya unexpected behavior ka exact reason dhoondhne ke liye use hota hai.
            (Ishko bad ma padhengen advance level par)
    
## 1.5 Shared Libraries
Shared Library ek aisi library file (.so) hoti hai jiska code ek hi jagah disk aur memory me store hota hai aur usse ek hi time par bahut saare programs share karke use kar sakte hain. Ya other files ka trah he disk par stored rehti hai. 
- **Purpose**
1. Ek hi library code ko multiple programs ke beech share karne ke liye Shared Libraries use ki jati hain.
2. Har executable ke andar same functions ka code copy karne se bachne ke liye Shared Libraries use ki jati hain.
3. Disk space bachane ke liye Shared Libraries use ki jati hain kyunki library code alag file me rakha jata hai.
4. RAM bachane ke liye Shared Libraries use ki jati hain kyunki ek hi library ko kai processes memory me share kar sakte hain.
5. Code reuse karne ke liye Shared Libraries use ki jati hain taaki ek baar likha gaya code kai programs use kar saken.
6. Library ko update karna aasaan banane ke liye Shared Libraries use ki jati hain, kyunki library alag file me hoti hai.
7. Executable file ka size chhota rakhne ke liye Shared Libraries use ki jati hain.
8. Program ko modular aur maintainable banane ke liye Shared Libraries use ki jati hain.

## 1.6 Dynamic Linking/loader
### 1. Dynamic Section
Ya ELF file ka ek section hota hai (.dynamic) joki ek special data area hota hai jo runtime dynamic linker ko btata hai ki program ko chalane ka lia kaun-kaun sa shared libraries cahiye, unke symbols kaisa resolve honge aur PLT/GOT jaisa relocation aur linking mechanisms ko kaisa handle karna hai. Basically ya runtime linking ka control center hota hai. 
- **Functional Categories of Dynamic Entries**
1. Shared Libraries info
2. Symbol & String Tables
3. Hash Tables
4. Relocation info
5. PLT/GOT related
6. Initialization/Finalization
7. Loading/Memory info
8. Versioning
9. Search Path
### 2. NEEDED Entries
### 3. Shared Library Search
### 4. Shared Library Mapping
### 5. Symbol Table
### 6. Dynamic Symbol Table
### 7. Symbol Resolution
### 8. Relocation
### 9. GOT
### 10. PLT
### 11. Lazy Binding
### 12. Eager Binding
### 13. Runtime Symbol Resolution Flow
### 14. Complete Dynamic Linking Execution Flow

## complete execution flow
