
## 1. Definition:
Input/Output (I/O) means moving data between the CPU/memory and external devices (keyboard, disk, network, printer).

## 2. Types of Input/Output Operations

1. **Programmed Input/Output**  
   Programmed I/O is a method in which the CPU directly controls and continuously checks the I/O device to perform input or output operations, so the CPU stays busy until the data transfer is complete.  
   *(In this method the CPU itself sits near the I/O device and repeatedly checks “Has the data arrived?”.)*

2. **Interrupt‑driven I/O**  
   Interrupt‑driven I/O is a method in which the CPU starts the I/O operation and continues other work; the I/O device interrupts the CPU only when it is ready, reducing CPU waiting time.  
   *(In this method the CPU does not wait; it says “Call me when the data is ready.”)*

3. **Direct Memory Access (DMA)**  
   DMA is a technique in which data is transferred directly between an I/O device and main memory without continuous CPU involvement, making data transfer very fast and efficient.  
   *(In this method the CPU steps aside and the I/O device talks directly to RAM.)*

---

## 3. I/O Organization and Systems

### 3.1 Role of Input/Output Operations in Computing Systems
1. **Data Transfer Between CPU and Peripherals** – I/O operations allow the CPU to receive data from input devices and send data to output devices.
2. **Bridging the Speed Gap** – Most devices are slower than the CPU. I/O operations help synchronize the fast CPU with slower devices using techniques like buffering, caching, or DMA.
3. **Support for Different Types of Devices** – I/O operations provide a standard method to communicate with various devices, each having different data formats and speeds.
4. **Enabling Program Execution and Interaction** – Programs often require user input or must produce output. I/O operations allow interaction between software and the real world.
5. **System Control and Status Monitoring** – Some I/O operations are used to control devices or check their status.

### 3.2 Difficulties in I/O Organization
1. **Speed Mismatch between CPU and I/O Devices** – The CPU works very fast, while I/O devices like keyboards, disks, and printers are much slower.
2. **Device Variety and Different Data Formats** – Each I/O device works in a different way and uses different data formats.
3. **Synchronization Problems** – CPU and I/O devices must be properly synchronized to avoid data loss or corruption.
4. **Interrupt Handling Overhead** – Interrupts are used to notify the CPU, but too many interrupts can reduce system performance.
5. **Error Detection and Recovery** – I/O devices are more error-prone; detecting errors and recovering data correctly increases system complexity.

### 3.3 I/O System Components
1. **I/O Devices** – These are the actual external devices that provide input to the system or receive output from it.
2. **Device Controller** – A device controller is a small hardware unit that controls a specific I/O device. It converts CPU commands into device-specific signals, manages data transfer, buffering, and error detection. Each device has its own controller.
3. **I/O Ports/Interface** – These are special registers through which the CPU communicates with device controllers. They hold data, show device state, and send commands to the device. It provides a communication path between the CPU and the controller.
4. **System Bus** – The system bus carries information between the CPU, memory, and I/O devices. It transfers actual data, specifies device or memory locations, and carries control signals.
5. **I/O Instructions** – Special CPU instructions used to perform input and output operations. Used to access I/O ports or memory-mapped I/O. They allow the CPU to control and access I/O devices.
6. **Interrupt System** – It allows I/O devices to notify the CPU when they need attention.
7. **DMA Controller** – A hardware unit that allows devices to transfer data directly to/from memory without CPU involvement.
8. **Device Drivers** – These are software programs that control I/O devices. They translate OS commands into device-specific instructions and provide hardware independence.

### 3.4 Importance of Input/Output Organization
1. It allows the computer to communicate with the outside world.
2. It reduces the load on the CPU.
3. It improves overall system performance.
4. It manages different speeds of devices.
5. It supports multitasking and multi-user systems.

## 4. Accessing I/O Devices
**Definition** – It means the method by which the CPU communicates with I/O devices to send data to them or receive data from them.

### 4.1 Components Involved in Accessing I/O Devices:
1. **CPU** – Executes instructions and issues I/O instructions.
2. **I/O Device** – Provides input or takes output.
3. **I/O Controller/Interface** – This is the mediator between the CPU and the device; it handles speed mismatch and performs data buffering/translation.

### 4.2 Internal Components of I/O Controller:
1. **Control Unit** – Decodes CPU commands and generates device-specific signals.
2. **Data Buffer** – Temporarily stores data between the CPU and device and handles speed mismatch.
3. **Status Register** – Holds device ready/busy/error flags.
4. **Control/Command Register** – Commands are stored here.
5. **Interface Logic** – Connects to the system bus and handles device electrical protocol signals.

### 4.3 Methods of Accessing I/O Devices

#### 1) Programmed I/O (Polling)
This is a technique in which the CPU directly controls and performs all data transfers between memory and an I/O device by continuously checking the device status through a program loop. In this method, the CPU remains busy waiting until the I/O device becomes ready and completes the data transfer itself without using interrupts or DMA.

**Mechanism:**
1. CPU checks the status register of the I/O controller.
2. If the device is busy, the CPU keeps waiting.
3. When the device becomes ready, the status bit changes.
4. CPU reads/writes data from/to the data register.
5. Data is stored in memory or sent to the device.
6. CPU moves to the next instruction.

**Advantages:**
1. Simple to understand and easy to implement.
2. Does not require complex hardware support.
3. Suitable for small and slow I/O devices.

**Disadvantages:**
1. The CPU remains busy while waiting for the I/O operation to complete.
2. CPU time is wasted due to continuous polling.
3. System performance becomes very low for large data transfers.

#### 2) Interrupt Driven I/O
This is an I/O technique in which the CPU initiates an I/O operation and then continues executing other tasks, while the I/O device notifies the CPU using an interrupt signal when it becomes ready or when the data transfer is complete.

**Mechanism:**
1. CPU gives a command to the I/O device.
2. CPU continues to execute the normal program.
3. The device completes its task.
4. The device generates an interrupt signal.
5. CPU pauses the current task.
6. CPU executes the Interrupt Service Routine (ISR).
7. Data is transferred to memory.
8. CPU returns to the previous program.

**ISR (Interrupt Service Routine)** – This is a special small program that runs when an interrupt occurs, handles data transfer, and returns the CPU to normal execution.

**Advantages:**
1. The CPU can perform other tasks while the I/O device is working.
2. It improves CPU utilization compared to programmed I/O.
3. It is more efficient for devices that work at moderate speeds.

**Disadvantages:**
1. Interrupt handling adds overhead to the CPU.
2. Frequent interrupts can reduce system performance.
3. Hardware and software design become more complex.

#### 3) Direct Memory Access (DMA)
In this technique, a special hardware unit called the DMA controller transfers data directly between an I/O device and main memory without the continuous involvement of the CPU.

**How it happens:**
- CPU says: "Transfer this much data from here to there."
- DMA controller performs the data transfer itself.
- CPU continues its own work.
- Upon completion of the transfer, the CPU receives an interrupt.

**Mechanism:**
1. CPU tells the DMA controller the source address, destination address, data size, and transfer direction.
2. CPU gives the start command to the DMA.
3. DMA controller takes bus control from the CPU.
4. DMA transfers data directly between memory and the device.
5. Transfer is completed.
6. DMA controller sends an interrupt to the CPU.
7. CPU continues normal execution.

**Advantages:**
1. DMA allows high-speed data transfer without CPU involvement.
2. The CPU is free to execute other programs during data transfer.
3. It is very efficient for large data blocks.

**Disadvantages:**
1. DMA hardware is complex and costly.
2. Cache coherence problems may occur.
3. It is inefficient for small data transfers.

---

## 5. Types of I/O Addressing Methods
There are two types of I/O addressing methods:

#### 1) Memory Mapped I/O (MMIO)
In this method, I/O devices share the same address space as main memory, so the CPU accesses devices using normal memory instructions just like accessing RAM.

**Mechanism:**
1. The CPU has a single address space.
2. A portion of this address space is for RAM, and another portion is reserved for I/O devices.
3. When the CPU performs a read/write at a specific address, if it is a RAM address, RAM is accessed; if it is an I/O address, the device is accessed.
- The CPU does not even know whether it is accessing RAM or a device because the instructions are the same.

**Advantages:**
1. Easy to program because the CPU uses normal memory instructions.
2. Allows the use of a rich instruction set, making data processing faster and more flexible.
3. Provides faster access because it works well with modern CPU pipelines.
4. Widely used in modern processors and embedded systems.

**Disadvantages:**
1. Reduces the available main memory address space.
2. Cache management becomes difficult because device registers should not be cached.
3. Hardware design is more complex due to shared address space.

#### 2) Isolated I/O
In this method, devices have a separate address space from main memory and the CPU accesses them using special I/O instructions.

**Mechanism:**
1. The CPU has separate address spaces for memory (RAM) and I/O (Ports).
2. I/O devices are assigned port numbers.
3. Memory is accessed via memory instructions, and I/O is accessed via special I/O instructions.

**Advantages:**
1. Does not consume main memory address space.
2. Clearly separates memory operations from I/O operations.
3. Cache problems do not occur because I/O uses a separate address space.
4. Hardware control logic is simpler.

**Disadvantages:**
1. Programming is harder because special I/O instructions are required.
2. The number of I/O instructions is limited compared to memory instructions.
3. Data transfer is generally slower than memory-mapped I/O.

---

## 6. Hardware Interrupts
### 6.1 Definition 
This is an interrupt sent by external hardware devices to the CPU when they require the CPU's attention.

### 6.2 Purpose:
- To identify and indicate the interrupt request from devices.
- To notify the CPU of pending interrupts.
- To prioritize multiple interrupt requests.
- To allow the CPU to process interrupts systematically by calling appropriate service routines.

### 6.3 Hardware Components of Interruption:
1. **Interrupting Device** – The hardware device that sends the interrupt signal (e.g., Keyboard, Mouse).
2. **IRQ (Interrupt Request Line)** – A physical signal line through which a device sends a request to the CPU or interrupt controller.
   - **Dedicated IRQ:** One device = one line.
   - **Shared IRQ:** Multiple devices = one line.
3. **Interrupt Controller** – Manages multiple interrupts, decides priority, and forwards one selected interrupt to the CPU.
   - **IRR (Interrupt Request Register):** Tracks which interrupts are requesting.
   - **ISR (In-Service Register):** Shows which interrupt the CPU is currently handling.
   - **IMR (Interrupt Mask Register):** Decides which interrupts are blocked/disabled.
   - **Priority Resolver:** Selects the highest priority among multiple interrupts.
   - **Control Logic:** Handles interrupt acknowledgment and generates the vector number.
4. **IVT (Interrupt Vector Table)** – A table in memory that stores the address of the ISR for each interrupt.
5. **Interrupt Vector (Interrupt Number)** – A unique number/code that identifies the interrupt to locate the correct ISR in the IVT.
6. **CPU Interrupt Control Logic** – Internal hardware that detects requests, decides to accept/ignore, saves program state, transfers control to ISR, and resumes the program later.
7. **INTA (Interrupt Acknowledge Signal)** – A signal sent by the CPU after accepting an interrupt to acknowledge it.
8. **ISR (Interrupt Service Routine)** – A routine that handles the device's task, clears the interrupt, and returns control.
9. **Stack (Interrupt Context Saving)** – Saves program data during an interrupt and restores it to resume execution.
10. **Timer/Clock Interrupt Hardware** – Generates periodic interrupts.
11. **Bus Interface & Signal Lines** – Signals travel through the control bus.
12. **Power Failure Interrupt Circuit** – Detects power drops and sends an interrupt for safe shutdown.

### 6.4 Mechanism:
1. **Interrupt Request Generation:** Device generates a signal for attention.
2. **Interrupt Detection by CPU:** CPU checks for interrupts after every instruction.
3. **Interrupt Enable Check:** CPU verifies if interrupts are enabled; if disabled, it is ignored.
4. **Completion of Current Instruction:** CPU finishes the current instruction first.
5. **Save CPU Context:** CPU saves its state in the stack or memory.
6. **Interrupt Priority Resolution:** If multiple interrupts exist, the CPU handles the highest priority one.
7. **Fetch Interrupt Vector:** CPU gets the address from the IVT.
8. **Jump to ISR:** Control is transferred to the ISR start address.
9. **Execution of ISR:** ISR handles data read/write or status clearing.
10. **Clear Interrupt Request:** ISR clears the flag so the same interrupt doesn't repeat immediately.
11. **Restore CPU Context:** CPU restores registers, flags, and program counter.
12. **Return from Interrupt:** CPU returns to the main program via RET/IRET.
13. **Resume Normal Execution:** CPU continues exactly where it stopped.

---

### 6.5 Types of Hardware Interrupts
1. **Maskable Interrupts** – Interrupts that the CPU can choose to listen to or ignore. Used for less critical devices like Keyboard or Mouse.
   - **Mechanism:** Device sends signal -> CPU checks if enabled -> If enabled, it is accepted; if disabled, it is ignored -> CPU re-enables them after critical work.
2. **Non-Maskable Interrupts (NMI)** – Interrupts that the CPU can never ignore. Used for emergency situations like hardware failure or power loss.
   - **Mechanism:** Critical event occurs -> NMI signal sent -> CPU pauses immediately -> NMI handler executes -> System takes recovery action or shuts down safely.

### 6.6 Triggering Methods:
- **Edge Triggered Interrupt:** Triggered by a signal change (low to high or high to low).
- **Level Triggered Interrupt:** Remains active as long as the signal level (High or Low) is maintained.

---

### 6.7 Interrupt Priority
Mechanism to decide which interrupt is serviced first when multiple occur simultaneously.

**Priority Schemes:**
1. **Fixed (Static) Priority:** Permanent priority (e.g., Power failure = Highest).
2. **Programmable Priority:** Priority levels can be changed via software/OS.
3. **Daisy Chaining Priority:** Devices connected in a series; the one closest to the CPU has the highest priority.
4. **Parallel Priority:** Interrupts arrive in parallel; a priority encoder selects the highest.
5. **Software Based Priority:** Managed by OS scheduling.

**Priority Levels:**
- **Highest:** Power failure, Timer interrupt.
- **Medium:** Disk, Network.
- **Lowest:** Keyboard, Mouse.

**Interrupt Nesting:** A high-priority interrupt can interrupt a low-priority ISR.
**Priority Masking:** CPU temporarily blocks certain interrupt levels.

---

### 6.8 Managing Interrupts in Modern Systems
- **APIC (Advanced Programmable Interrupt Controller):** Used in multicore systems to manage and route interrupts to the correct core.
- **Interrupt Routing:** Directing the interrupt to the most suitable processor.
- **Interrupt Throttling:** Limiting frequency to prevent CPU overload.

**Advantages of Interrupt-Driven Hardware:**
1. CPU efficiency improves as it doesn't wait/poll.
2. Faster response to events.
3. Reduced CPU idle time.
4. Better system performance for multiple devices.
5. Power saving.

---

## 7. Direct Memory Access (DMA)
**Definition** – A hardware-based mechanism where an I/O device directly reads/writes RAM data without involving the CPU for every byte, improving system performance.

### 7.1 Core Components:
1. **CPU:** Brain of the computer; sets up the transfer and monitors completion.
2. **DMAC (DMA Controller):** Hardware unit that controls data transfer between memory and I/O.
3. **Main Memory (RAM):** Provides source/destination locations.
4. **I/O Device:** The source or destination of the data.
5. **System Bus:** The communication path.

### 7.2 How it Works:
1. **CPU initializes DMA:** Sets source, destination, size, and direction.
2. **DMA requests bus control:** Sends a bus request signal. CPU responds with Bus Grant (BG) and releases the bus.
3. **DMA starts transfer:** Moves data directly.
   - **Burst Mode:** Full block transferred at once; CPU is blocked.
   - **Cycle Stealing:** One byte/word per cycle; CPU uses the bus in between.
   - **Transparent Mode:** DMA only uses the bus when the CPU is idle.
4. **DMA updates memory and I/O:** Generates addresses and control signals, bypassing CPU read/write cycles.
5. **Transfer complete:** DMA raises an interrupt to notify the CPU.

### 7.3 Working Modes of DMA:
1. **Burst Mode (Block Transfer Mode):** Continuous sequence transfer. DMA sends bus request -> CPU grants -> Full block moves -> Interrupt sent.
2. **Cycle Stealing Mode:** Transfers one byte/word per bus cycle by "stealing" a cycle. CPU executes its work in other cycles.
3. **Transparent Mode:** DMA monitors the CPU and only transfers when the bus is free, ensuring zero interference.

### 7.4 Types of DMA Transfers:
1. **Memory to Memory:** Data transfer between two RAM locations.
2. **I/O to Memory:** Data from device (Disk, Network card) to RAM.
3. **Memory to I/O:** Data from RAM to device (Display, Printer).

### 7.5 Advantages of DMA:
1. CPU is freed from data transfer tasks.
2. High-speed transfer for large data.
3. Better CPU utilization.
4. Improved overall system performance.

### 7.6 Limitations of DMA:
1. Increased hardware complexity (requires a DMAC).
2. Inefficient for small data transfers due to setup time.
3. Potential memory access conflicts between CPU and DMA.
4. Increases system cost.

## 8. I/O Device Types
- **Character devices:** Transfer data one character (byte) at a time. Examples: keyboard, serial port. Used for streams and interactive I/O.  
- **Block devices:** Transfer fixed-size blocks (sectors). Examples: HDD, SSD. Used for file systems and random access storage.  
- **Network Interface Card (NIC):** Special device that sends/receives packets; behaves like both character and block in different layers.  

**Why type matters:** Block devices allow block-level caching and scheduling; character devices need line discipline and buffering. 

---

## 9. I/O Hardware Basics
- **Device controller:** Hardware that implements device protocol and connects device to bus.  
- **Device registers:** Small registers (status, command, data) the CPU reads/writes to control device.  
- **Ports & connectors:** Physical/electrical interfaces (USB, SATA, PCIe).  
- **Status/command semantics:** Typical flow — CPU writes command register, device sets status bits, device raises interrupt when done. 

---

## 10. Bus Fundamentals
- **Lines:** *Address lines* select device/memory; *data lines* carry payload; *control lines* carry read/write/interrupt signals.  
- **Bus width & timing:** Wider bus = more bits per cycle; timing defines when signals are valid.  
- **Arbitration & contention:** Multiple masters (CPU, DMA) need arbitration logic; contention reduces throughput and increases latency. 

---

## 11. Transfer Modes
- **Programmed I/O (PIO):** CPU actively reads/writes device registers; simple but CPU‑intensive.   
- **Polling:** CPU repeatedly checks device status; wastes cycles but simple.  
- **Interrupt‑driven I/O:** Device interrupts CPU when ready; reduces polling overhead.  
- **DMA (Direct Memory Access):** DMA controller transfers blocks between device and memory without CPU for high throughput and low CPU load. 

---

## 12. Memory‑Mapped I/O vs Port‑Mapped I/O
- **Memory‑Mapped I/O (MMIO):** Device registers appear in the CPU address space; normal load/store access works.  
- **Port‑Mapped I/O (Isolated I/O):** Separate I/O address space and special instructions (IN/OUT).  
**Tradeoffs:** MMIO is flexible and simpler for modern CPUs; port‑mapped can isolate address spaces. 

---

## 13. Buffering and Caching for I/O
- **Single buffer:** One buffer for transfer; simple but blocks producer/consumer.  
- **Double buffer:** Two buffers allow overlap of I/O and processing.  
- **Circular buffer:** Many-slot ring buffer for streaming data (audio, network).  
- **Cache role:** Page cache or device cache stores recent blocks to reduce device access and hide latency. 

---

## 14. Basic I/O Performance Metrics
- **Throughput:** Data per second (MB/s).  
- **Latency:** Time to complete one operation (ms or µs).  
- **IOPS:** I/O operations per second (important for small random I/O).  
- **Bandwidth & response time:** Bandwidth is sustained rate; response time is per‑request delay. Measure with tools (fio, iostat). 

---

## 15. Simple Device Examples and Behavior
- **Serial port:** Byte stream, uses UART, needs baud rate, parity, stop bits; often uses interrupts or DMA for high speed.  
- **Parallel port:** Older, multiple data lines; used for printers historically.  
- **Disk read/write flow:** OS issues block request → block scheduler orders requests → device driver programs controller → controller reads sectors into buffer → DMA moves data to memory → interrupt signals completion → OS updates page cache and returns to process. 

---

## 16. I/O Device Control and Status Registers

Most I/O devices use small internal registers to manage data flow and device state. These registers work together so the CPU and device coordinate operations reliably.

### 16.1 Register Types
- **Control Registers**  
  - Receive commands from the CPU (e.g., start operation, reset device).  
  - Configure device modes (e.g., baud rate for serial ports, transfer size).  

- **Status Registers**  
  - Indicate device conditions (e.g., ready, busy, error).  
  - Read by the CPU (polled) or examined by interrupt handlers to decide next steps.

- **Data Registers**  
  - Temporary holding places for input or output data.  
  - CPU reads from or writes to these registers to move actual payload bytes/words.

### 16.2 Typical Interaction Flow
1. **CPU issues command** — CPU writes a command or configuration into the control register.  
2. **Device prepares** — Device updates its status register (e.g., sets Busy = 1 while preparing).  
3. **CPU checks readiness** — CPU polls the status register or waits for an interrupt indicating Ready.  
4. **Data transfer** — CPU reads from or writes to the data register to move data.  
5. **Completion** — Device clears Busy and sets any error bits in the status register; it may raise an interrupt.

---

## 17. Handshaking and Data Transfer

Handshaking synchronizes communication between CPU and I/O devices, especially when they run at different speeds.

### 17.1 Basic Handshaking Process
- **Device signals ready/busy** — Device asserts a line or status bit when it can accept or provide data.  
- **CPU checks signal** — CPU polls the signal or waits for an interrupt before starting transfer.  
- **Mutual readiness** — Transfer proceeds only when both sender and receiver indicate readiness.

### 17.2 Common Signals
- **Ready** — Device is ready to send or receive data.  
- **Acknowledge (ACK)** — Receiver confirms successful receipt or readiness to continue.  
- **Busy / Not Ready** — Receiver cannot accept data now; sender must wait or retry.

### 17.3 Data Transfer Techniques
- **Synchronous Transfer**  
  - Both sides share a clock or strict timing.  
  - Data is transferred on clock edges; low overhead for timing but requires clock alignment.

- **Asynchronous Transfer**  
  - No shared clock; uses handshaking signals (Ready/ACK) to coordinate timing.  
  - More flexible for devices with different speeds; slightly higher protocol overhead.

### 17.4 Variants and Enhancements
- **Burst transfers** — Transfer multiple words/bytes in one handshake to reduce per-item overhead.  
- **Scatter‑gather** — Device or DMA uses a list of memory segments to transfer noncontiguous buffers efficiently.  
- **Flow control** — Software or hardware signals (XON/XOFF, RTS/CTS) prevent buffer overrun in streaming links.
