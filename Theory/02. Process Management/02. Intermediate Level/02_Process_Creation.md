# 1. ELF Executable Format  
ELF (Executable and Linkable Format) ek standard executable file format hai jo machine code, program metadata aur loading information ko ek fixed structure ma organize karta hai taki linux kernel usshe memory ma load karke execute kar sake. 

Jab compiler aur linker source code ko machine code ma convert karte hai, tab linker us machine code aur required metadata ko ELF format ma package karta hai aur bani hui file SSD/HDD par ek normal file ki tarah store ho jata hai. 

## 1.1 Internal Structure 
1. ELF Header - Ya ELF file ka identity card hota hai jo batata hai ki ye ELF hai, architecture kya hai, entry point kya hai etc.
2. Program Header Table - Kernel ko btata hai ki file ka kaunse parts memory ma load karna hai aur kahan load karne hai.
3. .text - Program ka actual machine code (CPU instructions) yahan hota hai.
4. .rodata - Ya read only data hota hai.
5. .data - Ya initialized global/static variables hote hai.
6. .bss - Ya uninitialized global/static variables hote hai.
7. Symbol table - Ya functions aur variables ka names ki information hota hai. Ex - main, printf, scanf.
8. Dynamic Linking Information - Kaunsi shared libraries cahiye wo cheez store hota hai yaha par.
9. Section Header Table - Debugger, linker aur development tools ko btata hai ki sections kahan hai. 

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

2. readelf
3. objdump
4. nm
5. strings
6. size
7. ldd
8. hexdump
9. xxd
10. strace
11. ltrace
12. gdb
    
