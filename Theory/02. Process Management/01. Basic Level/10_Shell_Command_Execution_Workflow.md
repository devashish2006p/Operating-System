1. Command Input: User terminal mein command type karta hai aur Enter press karta hai, jiske baad terminal input shell ko provide karta hai.
2. Input Reading: Shell input ko read karta hai aur newline milne par command ko execute karne ke liye process karna shuru karta hai.
3. Lexical Analysis: Shell input ko tokens mein divide karta hai, jaise command name, arguments, operators aur redirection symbols.
4. Syntax Parsing: Shell tokens ki structure check karta hai aur identify karta hai ki command simple command hai, pipeline hai, ya usmein redirection aur control operators hain.
5. Expansion: Shell context ke according variable expansion, command substitution, arithmetic expansion, tilde expansion aur pathname expansion perform karta hai.
6. Quote Removal: Shell syntax ke liye use kiye gaye quotes aur escaping characters ko appropriate stage par remove karta hai, taaki command aur arguments ke final values determine ho saken.
7. Command Identification: Shell check karta hai ki command shell ka built-in command hai, function hai, ya external executable program hai.
8. Command Resolution: Agar command external hai, toh shell PATH environment variable mein diye gaye directories ko search karke executable locate karne ki koshish karta hai; agar command mein / hai, toh us path ko use karta hai.
9. Execution Preparation: Shell final argument list (argv), environment variables (envp) aur input/output redirections ke liye required setup prepare karta hai.
10. Process Creation or Reuse: External command ke liye aam taur par shell fork() ya clone() jaise mechanism se child process create karta hai, jabki kuch implementations posix_spawn() use kar sakti hain; shell khud bhi kuch situations mein exec se replace ho sakta hai.
11. Redirection Setup: Agar command mein redirection hai, toh relevant process open(), dup2() aur close() jaise system calls se standard input, output ya error streams ko configure karta hai.
12. Executable Loading Request: Child process execve() ya related execution mechanism call karta hai, jisse kernel executable file ko run karne ki request process karta hai.
13. Kernel Validation and Loading: Kernel file access permissions, executable format aur required loading information validate karta hai; valid ELF executable ke segments process ke virtual address space mein map kiye jaate hain, aur zaroorat padne par dynamic linker bhi start hota hai.
14. Program Startup: Executable ka entry point reach hone ke baad runtime startup code initialize hota hai aur program ko command-line arguments aur environment variables available karata hai.
15. Program Execution: Program apni instructions execute karta hai, input process karta hai aur configured output streams par results produce karta hai.
16. Process Termination: Program return ya exit ke through terminate hota hai, aur kernel uska exit status record karta hai.
17. Shell Resumption: Agar shell ne command ko foreground mein launch kiya tha, toh woh process ke complete hone ka wait karke control resume karta hai, exit status collect karta hai aur next command ke liye prompt display karta hai.
