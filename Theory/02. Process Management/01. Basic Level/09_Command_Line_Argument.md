# 1. Command Line Argument Representation
Command-Line Argument Representation ka matlab hai command run karte waqt program ko diye gaye arguments ko memory mein represent aur organize karna, taaki program unhe access kar sake; Linux mein program ke startup par arguments argc (arguments ki count) aur argv (arguments ke strings ki array) ke through access kiye ja sakte hain.

# 2. How it works
1. User terminal mein command likhta hai, jaise ./program hello 123.
2. Shell command ko parse karke executable file aur uske arguments identify karta hai.
3. Shell execve() system call ke through OS se executable program run karne ko kehta hai aur arguments pass karta hai.
4. Linux executable ko load karta hai aur startup ke time arguments ko process ke user-space memory mein arrange karta hai.
5. Program ka C runtime startup code arguments ki count ko argc aur arguments ke pointers ko argv ke through available karata hai.
6. Program argv[0] mein aam taur par program ka naam/path, argv[1] mein "hello" aur argv[2] mein "123" access kar sakta hai.
7. Program in arguments ko apni logic ke according process karta hai, jaise filename lena, calculation karna ya koi option select karna.

