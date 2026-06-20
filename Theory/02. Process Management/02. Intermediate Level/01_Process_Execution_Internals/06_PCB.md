# 1. PCB Creation Flow
## Phase 1 : Process Creation Request
1. Process creation ki request aati hai.
2. Shell fork() call karta hai.
3. fork() system call invoke karta hai
4. CPU user mode-> kernel mode switch karta hai.
5. Control kernel ka fork() handler ko milta hai.
6. Kernel naya process create karta hai.

## Phase 2 : Kernel PCB create karne ka decision leta hai. 
1. Kernel Verify karta hai ki naya process create karna allowed hai ya nahi.
    - Process limit
    - User Permissions
    - Available resources
2. Agr check fail ho gya to error return.
3. Agr Pass ho  gya to kernel naya process object create karne ka decision leta hai. 

# 2. PCB Life Cycle


# 3. PCB and Context Switching


# 4. PCB and Scheduling


# 5. PCB and Process States


# 6. PCB and Memory Management


# 7. PCB and I/O Management


# 8. PCB Storage


# 9. PCB vs Process


# 10. Real Execution Flow
