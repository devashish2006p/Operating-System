
## 1. Definition & Purpose of Operating System

### Definition
An Operating System (OS) is a system software that works as an intermediary between the user and computer hardware.  
It manages hardware resources, provides an environment to run programs, and offers services to users and applications.

### Purpose
The main purpose of an OS is:
- Efficiently manage hardware resources
- Make interaction between user and hardware simple, secure, and effective

---

## 2. Functions of an Operating System

### Introduction
The OS manages all computer resources like CPU, memory, files, and devices.  
It provides a simple and secure environment for users and software.

### OS functions can be understood from two points of view:

#### i) User’s Point of View
- Comfortable interface
- Security
- Ease of use

#### ii) System’s Point of View
- Efficient hardware resource management

---

## 3. Components of Operating System

### Introduction
An OS is a complex software system where each component has a specific role.  
All components together coordinate hardware and software.

---

### i) Kernel

The Kernel is the core of the OS and directly interacts with hardware.  
It controls low-level system operations.

**Kernel = Brain of the OS**

#### Functions of Kernel
- **Process Management** – Create, schedule, terminate processes
- **Memory Management** – Allocate/deallocate RAM, paging, segmentation, virtual memory
- **Device Management** – Handle drivers, interrupts, I/O requests
- **File Management** – Maintain metadata, allocation, permissions
- **System Call Handling** – Bridge between user programs and hardware

#### Types of Kernels
- **Monolithic Kernel** – All services inside one large kernel (fast, less modular)
- **Microkernel** – Only core services in kernel, others in user space (secure, slower)
- **Hybrid Kernel** – Combination of monolithic and microkernel
- **Exokernel** – Direct hardware access for applications (research based)
- **Nano Kernel** – Extremely small kernel, minimal services

---

### ii) Shell (Command Interpreter)

Shell is an interface between user and kernel.

**Shell = Translator between User and Kernel**

#### Functions
- Take commands from user
- Interpret commands
- Send system calls to kernel
- Return output

#### Types of Shell
- **CLI** – Text based (Bash, CMD)
- **GUI** – Graphical (GNOME, Windows Explorer)

---

### iii) File System

File System defines how data is stored, organized, and accessed on disk.

**File System = Logical structure of data storage**

#### Functions
- File create, delete, read, write
- Directory management
- Metadata handling
- Access control & permissions
- Space allocation & disk scheduling

---

### iv) Device Drivers

Device Driver is a software module that acts as a translator between OS and hardware.

**Driver = Hardware Translator**

#### Functions
- Convert OS commands to hardware signals
- Handle interrupts
- Control data flow between device and CPU

#### Types of Drivers
- Character Drivers (keyboard, mouse)
- Block Drivers (HDD, SSD)
- Network Drivers (Ethernet, Wi-Fi)
- File System Drivers (NTFS, FAT32)
- Virtual Drivers (VM devices)
- Filter Drivers (antivirus, encryption)
- Bus Drivers (USB, PCI)

---

### v) System Programs

System programs help users perform system-level tasks.

#### Categories
- File management programs
- Status information programs
- File modification programs
- Programming support programs
- Program loading & execution
- Communication programs
- Application programs

---

### vi) System Calls

System call is a method used by user programs to request kernel services.

**System Call = Gateway between user mode and kernel mode**

#### Categories
- Process control
- File management
- Device management
- Information maintenance
- Communication

---

### vii) User Interface
Medium through which user interacts with OS.

- CLI
- GUI

---

### viii) Daemon / Service Manager

Manages background services from boot to shutdown.

**Examples**
- Network services (SSH, DNS)
- Logging
- Updates
- Auto restart on crash

---

### ix) Virtual File System (VFS)

VFS provides a common interface to access different file systems.

**Role**
- Handles FS mismatch
- Uniform access for USB, HDD, SSD
- Generic file operations

---

### x) Network Stack

Manages all networking tasks.

**Role**
- IP assignment
- Packet transmission
- Firewall rules
- Routing
- Socket communication

---

### xi) Resource Control (Cgroups / Scheduler Extensions)

Controls CPU, memory, and I/O usage.

**Role**
- CPU limits
- Priority handling
- Resource isolation

---

### xii) Boot Loader

Loads OS into memory and transfers control to kernel.

**Role**
- BIOS/UEFI control
- Kernel loading
- Boot menu
- Multi-boot handling

---

## 4. Types of Operating Systems

- Batch OS
- Time Sharing OS
- Distributed OS
- Network OS
- Real Time OS (RTOS)
- Mobile OS
- Multiprogramming OS
- Multitasking OS
- Multiprocessing OS
- Multiuser OS
- Embedded OS
- Cloud OS
- Virtual Machine / Hypervisor OS
- Object Oriented OS
- Server OS
- Desktop OS
- Cluster OS
- Unix-like OS

---

## 5. OS Structure

### Types of OS Structures
- Monolithic
- Layered
- Microkernel
- Modular / Hybrid
- Client-Server
- Exokernel
- Virtual Machine

---

## 6. System Calls – Workflow

1. Application requests service
2. Library function triggers system call
3. Switch from user mode to kernel mode
4. Kernel performs task
5. Result returned to application

---

## 7. OS Services

- Program execution
- I/O operations
- File system services
- Communication
- Error detection
- Resource allocation
- Accounting
- Protection & security
- User interface
- Networking
- Device management
- Memory management

---

## 8. System Boot Process

1. Power on
2. CPU reset & first instruction
3. BIOS/UEFI initialization
4. POST
5. Boot device selection
6. Boot loader execution
7. Kernel loading
8. Initramfs
9. Root filesystem mount
10. Init / PID 1 start
11. Services start
12. Login / GUI

---

## END OF NOTES
