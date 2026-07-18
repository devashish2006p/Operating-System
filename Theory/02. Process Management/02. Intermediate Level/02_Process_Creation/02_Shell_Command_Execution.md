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
4. Validate – Syntax sahi hai ya nahi, check karti hai.
5. Identify – Built-in hai ya external, decide karti hai.
6. Locate – Agar external hai to executable locate karti hai.
7. Prepare – Execution ke liye required information ready karti hai.

## 3. Shell required System calls karta hai. 

## 4. Shell kernel sa response/exit status receive karke user ko output dikhata hai. 
