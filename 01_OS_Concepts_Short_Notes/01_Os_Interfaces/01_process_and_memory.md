# Operating System Overview & UNIX System Calls

## 1. Operating System kya hota hai?

Operating System (OS) ek aisa software layer hai jo **hardware aur user applications ke beech bridge** ka kaam karta hai.

Computer ko broadly is tarah samajh sakte hain:

```text
User Applications
│
├── Shell
├── Compiler
├── Database
├── Browser
└── Other Programs
        │
        │ System Calls
        ▼
+----------------------+
|        KERNEL        |
|----------------------|
| File System          |
| Process Management   |
| Memory Management    |
| Network Management   |
| Device Management    |
+----------------------+
        │
        ▼
Hardware
├── CPU
├── RAM
├── Disk
├── Network Card
└── Other Devices
```

User applications normally hardware ko directly control nahi karti hain. Agar kisi program ko file read karni hai, memory allocate karni hai, network par data bhejna hai, ya naya process banana hai, to woh **kernel se request karta hai**.

Ye request generally **system call** ke through hoti hai.

---

# 2. Operating System ka purpose kya hai?

## 2.1 Hardware ko multiple applications ke beech share karna

Computer par ek hi time par bahut saare programs chal sakte hain.

Example:

```text
Browser
Music Player
Terminal
VS Code
Database
```

Sabko CPU, RAM, disk aur network chahiye hota hai.

Operating system decide karta hai:

* CPU kis process ko milega.
* Kitni memory milegi.
* Disk access kaise hoga.
* Network device ko kaun use karega.

Is process ko broadly **hardware multiplexing** kaha ja sakta hai.

---

## 2.2 Applications ko isolate karna

Har application ko doosri application se isolate kiya jata hai.

Example:

```text
Program A crash
        │
        └── Ideally Program B ko affect nahi karega
```

Isolation ke benefits:

* Bugs contain hote hain.
* Security improve hoti hai.
* Ek application doosri application ki memory directly access nahi kar sakti.
* Malicious program ko unlimited hardware access nahi milta.

---

## 2.3 Applications ke beech data sharing

Kabhi-kabhi programs ko ek doosre ke saath communicate bhi karna hota hai.

Example:

```text
ls | grep txt
```

Yahan:

```text
ls → data produce karta hai
grep → us data ko receive karta hai
```

Operating system mechanisms provide karta hai, jaise:

* Files
* Pipes
* Sockets
* Shared memory

---

## 2.4 Hardware abstraction

Different computers ke hardware alag ho sakte hain.

Example:

```text
Computer A → Samsung SSD
Computer B → Western Digital SSD
Computer C → Different disk controller
```

Program ko har hardware ka internal implementation samajhne ki zarurat nahi hoti.

Program simply:

```c
read();
write();
open();
```

jaisi interfaces use karta hai.

Kernel internally actual hardware differences handle karta hai.

Isliye application kaafi had tak **portable** ban sakti hai.

---

# 3. Operating System design mein trade-offs

OS design mein har decision ke kuch advantages aur disadvantages hote hain.

## Efficient vs Abstract

Direct hardware access fast ho sakta hai, lekin difficult aur unsafe ho sakta hai.

Abstraction easy interface deta hai, lekin kabhi-kabhi extra overhead introduce karta hai.

---

## Powerful vs Simple Interface

Bahut powerful interface complicated ho sakta hai.

Bahut simple interface limited functionality de sakta hai.

OS designer ko dono ke beech balance banana padta hai.

---

## Flexible vs Secure

Agar system bahut flexible hai, to user ko zyada control mil sakta hai.

Lekin excessive freedom security risk bhi create kar sakti hai.

---

## Portable vs Hardware-specific optimization

Generic OS code multiple machines par chal sakta hai.

Lekin agar OS specific hardware features ka use kare, to performance better ho sakti hai.

Isliye portability aur hardware optimization ke beech trade-off hota hai.

---

# 4. Applications OS se kaise interact karti hain?

Applications kernel se directly normal function call ke through interact nahi karti hain.

Woh **system calls** use karti hain.

Example:

```c
read();
write();
open();
fork();
exec();
pipe();
```

Ye functions application ko normal function calls jaise dikh sakte hain.

Lekin internally ye kernel mein transition karte hain.

Basic flow:

```text
User Program
     │
     │ System Call
     ▼
CPU switches to kernel mode
     │
     ▼
Kernel executes requested operation
     │
     ▼
Return result
     │
     ▼
User Program continues
```

---

# 5. xv6 kya hai?

xv6 ek chhota operating system hai jo MIT ke operating system course ke liye banaya gaya hai.

Ye UNIX se inspired hai.

```text
UNIX concepts
      │
      ▼
Linux / macOS jaise modern systems
      │
      ▲
xv6 simplified version
```

xv6 ka main benefit ye hai ki iska code chhota hai.

Isliye student theoretically poore operating system ke important parts ko padh aur samajh sakta hai.

Course mein xv6 ke do major roles hain:

1. OS mechanisms ko samajhne ke liye example.
2. Labs ke liye starting point.

xv6 RISC-V architecture ke liye bana hai aur generally QEMU emulator ke andar run kiya jata hai.

---

# 6. Example 1 — `read()` aur `write()`

Suppose ek program input se data read karta hai aur output par write karta hai.

Conceptually:

```text
Input
  │
  ▼
read()
  │
  ▼
Program Memory
  │
  ▼
write()
  │
  ▼
Output
```

Example:

```c
read(0, buffer, 100);
write(1, buffer, n);
```

Yahan:

```text
0 → Standard Input
1 → Standard Output
```

---

# 7. File Descriptor kya hota hai?

File Descriptor ya FD ek small integer hota hai jo kernel ke through kisi open resource ko represent karta hai.

Resource ho sakta hai:

* File
* Pipe
* Socket
* Device

Example:

```text
FD 0 → Standard Input
FD 1 → Standard Output
FD 2 → Standard Error
```

Ek process ke paas multiple file descriptors ho sakte hain.

Conceptually:

```text
Process
│
├── FD 0 → Keyboard / Input
├── FD 1 → Terminal / Output
├── FD 2 → Error Output
└── FD 3 → Some File
```

Important baat ye hai ki **FD sirf file ko represent nahi karta**.

FD pipe ya socket ko bhi refer kar sakta hai.

---

# 8. `read()` kaise kaam karta hai?

Basic form:

```c
read(fd, buffer, count);
```

Arguments:

### `fd`

Batata hai kis resource se data read karna hai.

### `buffer`

Memory location jahan received data store hoga.

### `count`

Maximum kitne bytes read karne hain.

`read()` requested bytes se kam bytes read kar sakta hai.

Lekin normally requested limit se zyada bytes nahi dega.

Return value:

```text
Actual bytes read
```

Ya error hone par:

```text
-1
```

---

# 9. UNIX I/O byte-oriented hota hai

Kernel ko generally data ka meaning nahi pata hota.

Kernel ke liye data simply bytes hote hain.

Example:

```text
C source code
Database record
Image
Video
Text file
```

Kernel in sabko largely bytes ke form mein handle karta hai.

Actual interpretation application karti hai.

---

# 10. `open()` system call

Agar kisi file ko access karna hai, generally use open karna padta hai.

```c
int fd = open("file.txt", ...);
```

Kernel successful hone par ek file descriptor return karta hai.

Example:

```text
open("data.txt")
        │
        ▼
Kernel opens the file
        │
        ▼
Returns FD 3
```

Ab program:

```c
read(3, buffer, size);
```

kar sakta hai.

Har process ka apna FD namespace hota hai.

Isliye:

```text
Process A → FD 3 = file1.txt
Process B → FD 3 = socket
```

ho sakta hai.

Same FD number ka matlab different processes mein different resource ho sakta hai.

---

# 11. System call actually internally kaise kaam karta hai?

Program:

```c
open(...);
```

likhta hai.

Ye normal function call jaisa dikhta hai, lekin internally special CPU mechanism use hota hai.

Conceptually:

```text
User Program
      │
      │ System Call Instruction
      ▼
CPU saves required state
      │
      ▼
Privilege level changes
      │
      ▼
CPU jumps into Kernel
      │
      ▼
Kernel executes sys_open()
      │
      ▼
Kernel performs operation
      │
      ▼
Result prepared
      │
      ▼
CPU returns to User Mode
      │
      ▼
Program continues
```

Example `open()` ke case mein kernel:

* File system mein filename lookup karta hai.
* Required kernel data structures update karta hai.
* File descriptor allocate karta hai.
* Result user program ko return karta hai.

---

# 12. Shell kya hota hai?

Shell ek command-line program hai.

Example:

```text
$
```

Jo prompt tum terminal mein dekhte ho, woh shell provide karta hai.

Shell kernel nahi hota.

Shell ek **normal user-space program** hota hai.

Example commands:

```bash
ls
ls > out
grep x < out
```

Shell ka kaam broadly:

* Commands read karna.
* Programs run karna.
* Input/output redirect karna.
* Pipes create karna.
* Processes manage karna.

---

# 13. Process kya hota hai?

Program aur process same cheez nahi hain.

```text
Program = disk par stored executable code

Process = currently running program
```

Ek process ke paas generally hota hai:

* Instructions/code
* Data
* Memory
* Stack
* CPU state
* Kernel state
* File descriptors

Example:

```text
/bin/echo
```

ek program file hai.

Jab tum execute karte ho:

```bash
echo hello
```

to ek process create hota hai.

---

# 14. `fork()` — Naya process create karna

`fork()` system call ek new process create karta hai.

Original process:

```text
Parent Process
```

New process:

```text
Child Process
```

Conceptually:

```text
Parent
   │
   │ fork()
   ▼
 ┌─────────┐
 │ Parent  │
 └─────────┘
       +
 ┌─────────┐
 │ Child   │
 └─────────┘
```

Initially child parent ke execution state se related copy ke form mein create hota hai.

Lecture ke according ismein instructions, data, registers, file descriptors aur current directory jaise state copy hoti hai.

---

# 15. `fork()` return value

`fork()` dono processes mein return karta hai.

Parent mein:

```text
Child ka PID
```

Child mein:

```text
0
```

Example:

```c
int pid = fork();

if (pid == 0) {
    // Child process
} else {
    // Parent process
}
```

Isliye `fork()` ke baad dono processes same code ke next instructions execute kar sakte hain.

Lekin return value dekhkar pata lagaya ja sakta hai ki kaunsa parent hai aur kaunsa child.

---

# 16. PID kya hota hai?

PID ka full form hai:

```text
Process ID
```

Kernel har process ko ek identifier assign karta hai.

Example:

```text
Process A → PID 1200
Process B → PID 1201
Process C → PID 1202
```

PID ka use kernel aur programs process ko identify karne ke liye karte hain.

---

# 17. `exec()` — Process ke andar naya program load karna

`fork()` sirf process create karta hai.

Agar us process ko koi naya program run karwana hai, to `exec()` use kiya ja sakta hai.

Example:

```c
exec("echo", arguments);
```

Conceptually:

```text
Current Process
      │
      │ exec()
      ▼
Old Program Memory Removed
      │
      ▼
New Executable Loaded
      │
      ▼
Same Process continues with new program
```

`exec()`:

* Old instructions remove karta hai.
* Old program data remove karta hai.
* New executable load karta hai.
* New program execution start karta hai.

Important:

Successful `exec()` normally return nahi karta, kyunki purana program replace ho chuka hota hai.

---

# 18. `fork()` + `exec()` — Shell ka important mechanism

Shell ko har command ke liye khud replace nahi hona chahiye.

Suppose:

```bash
ls
```

Agar shell directly `exec(ls)` kar de, to shell khud `ls` ban jayega.

Uske baad next command lene ke liye shell nahi bachega.

Isliye shell broadly ye karta hai:

```text
Shell
 │
 │ fork()
 ▼
Parent Shell ───────────► wait()
 │
 │
 ▼
Child
 │
 │ exec("ls")
 ▼
ls Program Runs
 │
 ▼
exit()
 │
 ▼
Parent Shell continues
 │
 ▼
Next prompt
```

Basic flow:

```text
fork()
   ↓
Child → exec() → Run command
Parent → wait() → Wait for child
```

Ye UNIX process model ka bahut important pattern hai.

---

# 19. `wait()` aur `exit()`

Child process complete hone ke baad:

```c
exit(status);
```

call kar sakta hai.

Parent:

```c
wait(&status);
```

ke through child ka completion status receive kar sakta hai.

Conventionally:

```text
0 → Success
Non-zero → Error / failure indication
```

Isse parent process ko child ke execution result ke baare mein information mil sakti hai.

---

# 20. Copy-on-Write ka basic idea

Normal explanation se `fork()` wasteful lag sakta hai.

Example:

```text
fork()
→ Process memory copy

Immediately exec()
→ Copied memory discard
```

Modern systems is problem ko optimize karne ke liye **Copy-on-Write (COW)** jaise mechanisms use karte hain.

Basic idea:

```text
Parent aur Child initially same physical memory pages share kar sakte hain.

Jab koi process page modify karta hai,
tab actual copy create hoti hai.
```

Isse unnecessary copying avoid hoti hai.

---

# 21. I/O Redirection — `>`

Example:

```bash
echo hello > out
```

User ko lagta hai ki `echo` directly file mein write kar raha hai.

Actually shell file descriptors manipulate kar sakta hai.

Basic idea:

```text
Child Process
│
├── FD 0 → Standard Input
└── FD 1 → File "out"
```

Uske baad:

```text
Child → exec("echo")
```

`echo` simply FD 1 par write karta hai.

Usse pata nahi hota ki FD 1 terminal hai ya file.

Is concept ko **indirection through file descriptors** samajh sakte hain.

---

# 22. Redirection ka conceptual flow

Command:

```bash
echo hello > out
```

Shell broadly:

```text
Shell
 │
 ├── fork()
 │
 ▼
Child
 │
 ├── close standard output
 │
 ├── open "out"
 │
 └── exec echo
        │
        ▼
echo writes to FD 1
        │
        ▼
Data goes into "out"
```

Parent shell ke file descriptors disturb nahi hote.

Isliye child mein changes karke `exec()` karna useful hai.

---

# 23. File Descriptors ka major benefit

Program ko actual destination ka pata hona zaroori nahi hota.

Program simply:

```c
write(1, data, size);
```

karta hai.

FD 1 ho sakta hai:

```text
Terminal
File
Pipe
Socket
```

Program ka code same reh sakta hai.

Ye UNIX design ka powerful abstraction hai.

---

# 24. Pipe kya hota hai?

Pipe processes ke beech data transfer karne ka mechanism hai.

Example:

```bash
ls | grep x
```

Conceptually:

```text
ls
 │
 │ output
 ▼
PIPE
 │
 │ input
 ▼
grep x
```

Pipe kernel ke through manage hota hai.

`pipe()` system call generally do file descriptors provide karta hai:

```text
Read End
Write End
```

Conceptually:

```text
Process A
   │
   │ write()
   ▼
+----------------+
| Kernel Buffer  |
|     PIPE       |
+----------------+
   │
   │ read()
   ▼
Process B
```

---

# 25. Pipe internally basic level par

Writer:

```c
write(write_fd, data, size);
```

Kernel pipe buffer mein data store karta hai.

Reader:

```c
read(read_fd, buffer, size);
```

Pipe buffer se data receive karta hai.

Agar data available nahi hai, read operation wait kar sakta hai.

---

# 26. `pipe()` + `fork()` — Parent aur Child communication

Basic flow:

```text
Create Pipe
      │
      ▼
fork()
   /       \
Parent     Child
   │         │
 write      read
   │         │
   └── PIPE ─┘
```

Is tarah parent aur child processes communicate kar sakte hain.

---

# 27. Shell pipeline internally

Command:

```bash
ls | grep x
```

Conceptually shell multiple operations combine karta hai:

```text
pipe()
   │
   ▼
fork()
   │
   ├── Child 1 → connect stdout to pipe → exec(ls)
   │
   └── Child 2 → connect stdin from pipe → exec(grep)
```

Isliye UNIX ke simple abstractions:

```text
fork()
exec()
pipe()
file descriptors
```

milkar powerful functionality create karte hain.

---

# 28. Directory ko read kaise kiya jata hai?

Directory bhi file-system object hoti hai jo entries ke baare mein information contain karti hai.

Example:

```bash
ls
```

Directory ki entries read karke filenames obtain kiye ja sakte hain.

Conceptually:

```text
Directory
│
├── file1.txt
├── file2.c
├── program
└── folder
```

`ls` directory information read karke user ko names display karta hai.

---

# 29. `.` kya hota hai?

UNIX/Linux mein:

```text
.
```

current directory ko represent karta hai.

Example:

```bash
ls .
```

ka matlab hai:

```text
Current directory ke contents list karo
```

Example:

```bash
./program
```

ka matlab:

```text
Current directory mein jo "program" file hai usko execute karo.
```

---

# 30. Lecture 1 ka complete mental model

Is lecture ka sabse important flow ye hai:

```text
User types command
        │
        ▼
Shell receives command
        │
        ▼
Shell creates child using fork()
        │
        ├───────────────┐
        ▼               │
Child                  Parent
        │               │
        │ exec()        │ wait()
        ▼               │
Program Runs            │
        │               │
        ├── read()      │
        ├── write()     │
        ├── files       │
        ├── pipes       │
        └── other system calls
        │
        ▼
System Call
        │
        ▼
Kernel
        │
        ├── File System
        ├── Process Management
        ├── Memory Management
        ├── Network
        └── Hardware Management
        │
        ▼
Hardware
```

---

# 31. Sabse important concepts jo is lecture se samajhne hain

## Operating System

Hardware aur applications ke beech management aur abstraction layer.

## Kernel

OS ka privileged core jo critical services aur hardware interaction manage karta hai.

## User Space

Jahan normal applications run karti hain.

## System Call

User program ka kernel se service request karne ka controlled mechanism.

## File Descriptor

Open file, pipe, socket ya other I/O resource ko represent karne wala integer handle.

## Process

Running program with its own execution state and resources.

## PID

Process ka unique identifier.

## `fork()`

Naya child process create karta hai.

## `exec()`

Current process ke program ko naye executable se replace karta hai.

## `wait()`

Parent ko child process ke completion ka wait karne deta hai.

## `exit()`

Process terminate karta hai aur status provide kar sakta hai.

## Pipe

Processes ke beech byte-stream communication mechanism.

## Shell

User-space program jo commands run karta hai aur `fork()`, `exec()`, pipes aur redirection jaise mechanisms use karta hai.

---

# Final One-Line Flow

```text
User Command
→ Shell
→ fork()
→ Child Process
→ exec()
→ Program
→ System Calls
→ Kernel
→ Hardware
```

Aur agar programs ko communicate karna ho:

```text
Program A
   ↓ write()
   PIPE
   ↓ read()
Program B
```

**Lecture 1 ka core purpose:** tumhe ye samjhana ki UNIX/Linux mein programs kaise run hote hain, kernel se kaise interact karte hain, processes kaise create hote hain, aur files/pipes ke through I/O aur communication kaise hota hai.
