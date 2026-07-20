# 1. What is Shell?
Shell ek command interpreter program hai jo user ke commands ko samajhkar (interpret karke) system call karta hai aur operating system ke kernel se execute karwata hai aur uska result user ko wapas dikhata hai. Ya kudh ek Program hota hai. 

---
# 2. Categories of Shell 
1. Command Line Shell (CLI Shell) - Commands text ka form ma liye jate hai. Ex - Bash, Zsh, Fish, Ksh, Tcsh etc.
2. Graphical Shell (GUI Shell) - Mouse aur Graphical interface ka through interaction hota hai. Ex - GNOME Shell, KDE Plasma, Windows Explorer etc.

---
# 3. Terminal 
Terminal ek user interface program hai jo user sa keyboard ka through input leta hai, us input ko shell tak pahunchata hai aur shell sa aaya output screen par dikhata hai. 
## 3.1 How it works 
1. User keyboard sa koi command type karta hai aur enter press karta hai.
2. Keyboard sa aaya hua input terminal program receive karta hai aur apne input buffer me temporarily store karta hai.
3. Terminal typed characters ko live screen par display karta rehta hai taki user dekh sake ki usne kya type kiya hai.
4. Jab user enter press karta hai, terminal samajh jata hai ki command complete ho gui hai aur ab ise shell ko bhejna hai.
5. Terminal poori command ko exactly waisa he shell proces ko forward kar deta hai. Terminal command ka meaninng check ya interpret nhi karta hai.
6. Ab terminal wait state ma chala jata hai aur shell ke response ka intazar karta hai.
7. Shell command ko process karta hai aur execution ka baad output ya error generate karta hai.
8. Shell apna output terminal ko bhej deta hai.
9. Terminal output receive karke use screen par render kar deta hai.
10. Output dikhane ka bad terminal dobara user ka next command ka wait karne lagta hai. 

---
# 4. Shell Command Execution
## 1. User Command Enter karta hai
User Command Enter karna Shell Command Execution ka pehla step hai, jisme user Terminal ke through keyboard se koi command type karke Enter press karta hai taaki Shell us command ko process kar sake.

## 2. Shell command ko receive, identify (interpret) aur process karta hai. 
1. Receive – Shell command receive karti hai.
  - Terminal Enter press hone ke baad poori command Shell ko send karta hai.
  - Shell command data ko receive karti hai.
  - Shell received bytes ko apni process memory ke input buffer me copy/store karti hai.
  - Shell verify karti hai ki poori command receive ho chuki hai.
  - Ab receive phase khatam hota hai aur parsing phase shuru hota hai.
2. Store – Command ko temporarily memory me store karti hai.
3. Parse – Command ko todkar samajhti hai.
  - Shell stored command ko parsing engine ko deti hai.
  - Parser command string ko left se right read karna shuru karta hai.
  - Parser command ko logical parts (tokens) me todta hai (jaise command name, arguments, operators, redirections).
  - Parser har token ki type identify karta hai (command, argument, operator, etc.).
  - Parser token sequence ko syntax rules ke according verify karta hai.
  - Agar syntax error milti hai to parsing yahin stop ho jati hai aur Shell error return karti hai.
  - Agar syntax sahi hoti hai to parser parsed command structure prepare karta hai.
  - Ye parsed structure agle phase (Identification phase) ko handover kar diya jata hai.
4. Validate – Syntax sahi hai ya nahi, check karti hai.
  - Shell parsed tokens ko validation engine ko deti hai.
  - Validation engine command ki syntax ko shell grammar ke against check karti hai.
  - Har operator (|, >, <, &&, ; etc.) ki position verify karti hai.
  - Check karti hai ki required operands/arguments maujood hain ya nahi.
  - Check karti hai ki tokens ka sequence valid hai ya nahi.
  - Agar koi syntax error milti hai to validation turant stop ho jati hai.
  - Shell syntax error message prepare karti hai aur execution ko aage nahi badhati.
  - Agar sab valid hai to validated command structure ko Identification phase me bhej diya jata hai.
5. Identify – Built-in hai ya external, decide karti hai.
  - Shell parsed command structure se command name nikalti hai.
  - Shell apni built-in command list me us command ko search karti hai.
  - Agar built-in mil jata hai to command ko "Built-in" mark kar deti hai.
  - Agar built-in nahi milta to command ko "External Command" mark kar deti hai.
  - Agar external mark hua hai to agla phase (Location) start hota hai.
  - Agar built-in mark hua hai to Location phase skip ho jata hai aur Shell direct execution ke liye prepare karti hai.
6. Locate – Agar external hai to executable locate karti hai.
  - Shell command name ko Location module ko deti hai.
  - Shell executable search path (PATH) ko read karti hai.
  - Shell PATH me diye gaye directories ko ek-ek karke search karti hai.
  - Har directory me us command naam ki executable file ko check karti hai.
  - Jab matching executable mil jata hai, uska complete path determine karti hai (jaise /usr/bin/ls).
  - Verify karti hai ki file executable hai aur use run kiya ja sakta hai.
  - Executable ka full path execution preparation phase ko handover kar deti hai.
  - Agar executable nahi milta, to search stop karke "command not found" error prepare karti hai.
7. Prepare – Execution ke liye required information ready karti hai.
  - Shell executable ka full path receive karti hai.
  - Shell command ke arguments ko arrange karti hai.
  - Shell execution ke liye required information (path + arguments + environment) ko ek execution structure me organize karti hai.
  - Shell verify karti hai ki execution ke liye saari required information available hai.
  - Execution request ko final form me prepare karti hai.
  - Prepared execution request ko Step 3 (Required System Calls) ke liye handover kar deti hai.
## 3. Shell required System calls karta hai. 
Shell required system calls karti hai, yani user ki command ko execute karwane ke liye Kernel se zaroori services maangne ke liye appropriate system calls invoke karti hai.
## 4. Shell kernel sa response/exit status receive karke user ko output dikhata hai. 
Shell Kernel se command ka execution result aur exit status receive karti hai, phir us result ko process karke Terminal ko bhejti hai taaki user command ka final output ya error message dekh sake.

# 5. Shell Information Tools 
1. echo - echo ek shell built-in command hai jo kisi text, variable ki value ya escape sequence ko terminal ke standard output par display karta hai; internally shell echo command ko directly execute karke diye gaye arguments ko parse karta hai, variables ($VAR) ko expand karta hai aur final string ko output stream (stdout) me bhej deta hai; iska use tab kiya jata hai jab hume terminal ya shell script me koi information print karni ho, variable ki value check karni ho, ya debugging ke time shell ke andar ki state dekhni ho.
2. type - type ek shell built-in command hai jo ye identify karta hai ki diya gaya command shell ke andar built-in hai, alias hai, function hai ya external executable file hai; internally shell command lookup process ko use karke pehle apne environment me command ke type ko search karta hai aur phir uska source/path batata hai; iska use tab kiya jata hai jab hume samajhna ho ki koi command actually kaha se execute ho rahi hai, debugging karni ho ya command ke behavior ko verify karna ho.
3. which - which ek external command hai jo kisi command ke executable file ka exact location (path) find karta hai; internally ye system ke PATH environment variable me listed directories ko ek-ek karke search karta hai aur jaha pehli baar matching executable milta hai uska absolute path return karta hai; iska use tab kiya jata hai jab hume pata karna ho ki koi command system me kaha installed hai ya kaunsa executable version run ho raha hai.
4. whereis - whereis ek command-line utility hai jo kisi program ke binary executable, source code aur manual (man page) files ke locations ko find karta hai; internally ye predefined system directories aur database/standard paths me search karke command se related files ke possible locations return karta hai; iska use tab kiya jata hai jab hume kisi software/command ki installation files, binary path ya documentation ka location ek saath pata karna ho.
5. env/printenv - env / printenv ek command-line utility hai jo system ke environment variables ko display ya temporary environment ke saath command execute karne ka kaam karti hai; internally ye process ke environment block (memory area jaha shell ke variables jaise PATH, HOME, USER stored hote hain) ko read karke unki key-value pairs ko output karta hai; iska use tab kiya jata hai jab hume system environment ki configuration dekhni ho, kisi variable ki value check karni ho, ya program ko specific environment ke saath run karna ho.
6. echo $? - echo $ ka use shell me kisi variable ki value ko terminal par print karne ke liye hota hai, jaha $ shell ko signal deta hai ki uske baad likha gaya naam ek variable hai jisko expand karna hai; internally shell pehle $VARIABLE_NAME ko detect karta hai, apne variable table/environment se uski stored value lookup karta hai aur phir us value ko echo command ke argument ke roop me pass karke standard output (terminal) par display kar deta hai; iska use tab kiya jata hai jab hume kisi shell variable ya environment variable ki current value check, debug ya script ke andar use karni hoti hai.
7. strace - strace ek debugging aur system monitoring tool hai jo kisi running program ke dwara kiye ja rahe system calls aur signals ko trace karta hai; internally ye Linux ke ptrace() mechanism ka use karke process execution ko observe karta hai aur jab process kernel se koi service (jaise open(), read(), write(), execve(), fork()) request karta hai to us system call ka naam, arguments aur return value capture karke dikhata hai; iska use tab kiya jata hai jab hume samajhna ho ki koi program kernel ke saath kaise interact kar raha hai, error/debugging karni ho, file access, permission issue ya process behavior analyze karna ho.
8. bash - bash (Bourne Again SHell) ek command-line interpreter aur Unix/Linux shell hai jo user ke commands ko read, parse aur execute karta hai; internally ye user input ko tokens me divide karta hai, variables expand karta hai, command lookup karta hai aur phir zarurat ke according system calls (fork(), execve() etc.) ke through programs ko run karta hai; iska use tab kiya jata hai jab hume Linux system ke saath interact karna ho, commands execute karni ho, shell scripts chalani ho ya ek controlled shell environment test karna ho.
