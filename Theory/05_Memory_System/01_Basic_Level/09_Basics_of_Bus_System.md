# 1. System Bus 

A **system bus** is the communication pathway that connects the CPU, memory, and I/O devices.  
It is composed of three main types of lines:
- **Data lines**: Carry actual data (instructions or operands) between CPU, memory, and devices.  
- **Address lines**: Specify the memory location or I/O port involved in the transfer.  
- **Control lines**: Carry signals that define the type of operation (read/write), timing, and coordination.  

**Role:**  
- Acts as the backbone of the computer system.  
- Ensures CPU can fetch instructions, read/write data, and communicate with peripherals.  
- Performance of the bus directly impacts system speed and efficiency.

---

## 2. Bus Categories
- **System Bus**: The main bus connecting CPU, memory, and I/O. It integrates data, address, and control lines.  
- **Local Bus**: High-speed bus close to the CPU for fast devices (e.g., cache, graphics).  
- **Expansion Bus**: Allows additional cards (sound, network, graphics) to connect to the system. Examples: PCI, PCIe.  
- **Peripheral Buses**: Specialized buses for external devices. Examples: USB, SATA, I²C, SPI.  

**Hierarchy:**  
System bus → Local bus (fast internal devices) → Expansion bus (add-on cards) → Peripheral bus (external devices).

---

## 3. Control Signals :-  
  - **RD/WR**: Read or Write operation.  
  - **CS (Chip Select)**: Activates a specific device.  
  - **ALE (Address Latch Enable)**: Indicates when address lines are valid.  
  - **CLK (Clock)**: Synchronizes data transfer.  
  - **RESET**: Initializes devices to a known state.  

---

## 4. Bus Width and Word Size
- **Bus width** = number of data lines.  
- Common widths: 8, 16, 32, 64 bits.  
- **Impact:** Wider bus transfers more data per cycle → higher throughput.  
- **Word size:** CPU’s natural data size (e.g., 32-bit CPU has 32-bit word). Bus width should match or exceed word size for efficiency.  

---

## 5. Bus Timing and Cycles
- **Read/Write Cycle:** Sequence of signals to complete one transfer.  
- **Setup Time:** Minimum time data must be stable before clock edge.  
- **Hold Time:** Minimum time data must remain stable after clock edge.  
- **Propagation Delay:** Time taken for signal to travel across bus lines.  
- **Impact:** Timing defines maximum bus speed. Poor timing → errors or reduced performance.  

---

## 6. Synchronous vs Asynchronous Buses
- **Synchronous Bus:**  
  - Uses a common clock signal.  
  - All devices transfer data on clock edges.  
  - Fast and predictable, but requires strict timing alignment.  

- **Asynchronous Bus:**  
  - No common clock.  
  - Uses handshaking signals (Ready/ACK).  
  - Flexible for devices with different speeds, but slower due to handshake overhead.  

---

## 7. Transfer Modes
- **Single Transfer:** One word per bus cycle.  
- **Burst Transfer:** Multiple consecutive words transferred in one cycle (common in cache/memory).  
- **Pipelined Transfer:** Overlaps address and data phases to increase throughput.  

---

## 8. Basic Arbitration
When multiple devices want bus access:
- **Centralized Arbitration:** One controller decides who gets the bus (e.g., CPU or bus arbiter).  
- **Distributed Arbitration:** Devices negotiate among themselves.  
- **Fixed Priority:** Some devices always get preference (risk of starvation for low-priority devices).  

**Goal:** Ensure fairness and efficiency in bus usage.

---

## 9. Physical Layer Basics
- **Connectors:** Physical slots (PCIe, USB ports).  
- **Traces:** Copper lines on motherboard carrying signals.  
- **Impedance:** Resistance to signal flow; must be matched to avoid reflections.  
- **Termination:** Resistors or circuits at line ends to absorb signals and prevent reflection.  
- **Signal Integrity:** Ensures clean, reliable transmission; affected by crosstalk, noise, skew.  

---

## 10. Common Examples
- **ISA (Industry Standard Architecture):** Early PC bus, slow, parallel, legacy.  
- **PCI (Peripheral Component Interconnect):** Faster parallel bus, supports plug-and-play.  
- **PCIe (PCI Express):** Modern high-speed serial bus with lanes, scalable bandwidth.  
- **USB (Universal Serial Bus):** Widely used peripheral bus, supports hot-plugging and multiple speeds.  
- **I²C (Inter-Integrated Circuit):** Two-wire bus for low-speed communication between chips.  
- **SPI (Serial Peripheral Interface):** Four-wire bus, faster than I²C, used in embedded systems.  

---
## 11. Types of Bus
- There are 3 types of Buses:
1. **Data Bus:** This carries the actual data being transferred. It is **bi-directional**. Its width (8-bit, 16-bit, 32-bit, 64-bit) decides how much data can be transferred in a single clock cycle.
2. **Address Bus:** Through this, the CPU specifies where data needs to be read from or written to. It is **unidirectional** (CPU → Memory or I/O). Its width decides the computer's addressing capacity.
3. **Control Bus:** This carries signals that specify which operation to perform. All instructions are transmitted through this. It is generally considered **unidirectional** or a collection of individual timing signals.

---

## 12. Types of Bus Configuration

### i) Single Bus Architecture
This is a design where the CPU, memory, and I/O devices all share a single common communication line. Address, data, and control signals all travel through this same path.

**Components:**
* **Address Bus:** CPU specifies the memory or I/O location.
* **Data Bus:** Transfers the actual data.
* **Control Bus:** Manages Read/Write, clock, and interrupt signals.

> All components are on the same bus, which is why it is called "Single Bus" architecture.



**Working Flow:**
* When the CPU fetches an instruction:
    1. It sends the memory location on the **Address Bus**.
    2. It sends a "Read" signal on the **Control Bus**.
    3. The instruction arrives from memory via the **Data Bus**.
* The same bus is used during the execution phase for data transfer.
* **Constraint:** Only one operation can take place on the bus at a time.

---

### ii) Dual Bus Architecture
This is a structure where system components are connected through two separate buses. The main goal is to increase parallelism and speed so that memory access and I/O operations can happen simultaneously on different buses.

**Components:**
1. **Memory Bus:** Used for data transfer between the CPU and Main Memory. It supports high-speed communication.
2. **I/O Bus:** Used for communication between the CPU and I/O devices. It provides a dedicated bus for slower devices.
3. **Control Lines:** There are separate control signals for both buses.



**Mechanism:**
1. **Instruction Fetch (Memory Bus):** CPU fetches an instruction from main memory using the Memory Bus.
2. **Instruction Decode (CPU Internal):** The CPU decodes the instruction and decides if the operation is related to memory or I/O.
3. **Execution Phase:** If the instruction is to read/write memory, the Memory Bus is used. If it involves an I/O device, the I/O Bus is utilized.
4. **Parallelism:** In a dual bus system, memory and I/O operations can run at the same time. *Example:* The CPU can fetch the next instruction from memory while simultaneously sending data to a printer via the I/O bus.
5. **Control Signals:** Each bus has its own control signals; the CPU uses these to manage operations on each bus.
6. **Result Storage:** After execution, the result is either stored in memory or transferred to an I/O device.

---

### iii) Multiple Bus Architecture
This is a structure that contains more than two buses, where each bus is allocated for a specific purpose.

**Components:**
* **Instruction Bus:** For fetching instructions from memory to the CPU.
* **Data Bus:** For data transfer between CPU and memory.
* **I/O Bus:** For communication between CPU and I/O devices.
* **Control Lines:** Each bus has its own dedicated control signals.

---

## 13. Bus Performance Factors
1. **Bus Width:** The number of bits a bus can transfer in one cycle. Greater width means more data is transferred simultaneously.
2. **Bus Speed:** The frequency at which the bus operates (measured in MHz/GHz). It decides how many cycles occur per second.
3. **Bus Bandwidth:** Refers to the maximum data transfer capacity of the bus per second.
4. **Bus Arbitration:** When multiple devices want to access the same bus, arbitration decides which device gets priority.
    * **Centralized Arbitration:** A single bus controller makes the decision.
    * **Decentralized Arbitration:** Devices decide among themselves.
    * **Distributed Arbitration:** Multiple controllers work together to make the decision.

**Bus Contention:** A conflict that occurs when multiple devices demand to use the bus at the same time.

---

## 14. Types of Bus Structures

1. **System Bus:** The main communication highway that directly connects the CPU, Memory (RAM), and I/O modules. It is a combined group of the Data, Address, and Control buses.
2. **Memory Bus:** A specialized high-speed bus used specifically for data, address, and control signals between the CPU and RAM. It reduces latency and provides very high speeds.
3. **I/O Bus:** A pathway for CPU and external devices (Mouse, SSD, GPU, Printer, etc.). It connects devices that cannot connect directly to the high-speed memory bus.
4. **Backplane Bus:** A large, rigid PCB (Printed Circuit Board) with multiple connectors/slots. Multiple cards (CPU, Memory, I/O) share this single system bus by plugging into it.
5. **Expansion Bus:** A pathway that allows adding extra devices (Graphics card, Network card, Wi-Fi card) to the computer. It connects the motherboard to external/peripheral devices.

---

## 15. Steps in Bus Operations
1. **Request:** The device wanting to communicate sends a request to use the bus. Only one device can use the bus at a time. (e.g., CPU requesting memory or DMA requesting hard disk).
2. **Bus Arbitration:** The **Bus Arbiter** decides which device will use the bus next.
3. **Data Transfer:** The device that has been granted the bus performs its data operation.

---

## 16. Types of Data Transmission
1. **Synchronous Transmission:** Sender and receiver use a common clock. Data travels at fixed timings based on the clock "ticks."
2. **Asynchronous Transmission:** Sender and receiver do not share a clock. Instead, data is sent with **Start bits, Stop bits, and Parity bits**.
3. **Isochronous Transmission:** Data is sent in fixed timing slots. Even if there is an error or a packet is missed, the timing does not relax. Used for time-sensitive data; there is no retry or retransmission.

---

## 17. Bus Standards
These are official rules for designing hardware (Motherboards, CPUs, RAM, etc.) so that components from different companies are compatible.
* **PCI (Peripheral Component Interconnect):** Used for high-speed internal devices.
* **USB (Universal Serial Bus):** Used for external devices.
* **SATA:** Used for connecting storage devices.
* **I2C:** Used for low-speed communication in the microcontroller world.
* **CAN Bus:** Used for communication in cars, trucks, and industrial machines.

---

## 18. Bus Arbitration

Bus Arbitration is a decision-making mechanism whose job is to decide which device among multiple devices will use the shared system bus and in what order, when more than one device wants to access the bus at the same time.

---

## 19. Types of Bus Arbitration

### 1) Centralized Bus Arbitration

It is a technique in which a single central unit decides which device will get control of the bus and when it will get it.

#### Main Components

i) **Bus Arbiter (Central Controller)** – It is the decision maker.  
ii) **Bus Request Lines (BR)** – Devices send requests to the arbiter.  
iii) **Bus Grant Lines (BG)** – The arbiter gives permission to the device.  
iv) **Bus Busy Signal** – It indicates whether the bus is currently free or occupied.  

---

#### Working Mechanism

i) **Device needs bus** – The device sends a BR signal to the arbiter.  
ii) **Multiple requests possible** – The arbiter receives all requests.  
iii) **Priority Check** – The arbiter applies a priority scheme.  
iv) **Grant Signal** – The arbiter sends BG to the selected device.  
v) **Device Uses Bus** – The device performs data transfer.  
vi) **Release bus** – The device frees the bus.  

---

#### Priority Algorithms

##### i) Daisy Chain Arbitration

In this method, the bus grant signal passes serially through devices, and the device that captures the signal first gets control of the bus. It means the closest device has the highest priority.

###### Mechanism

1) Multiple devices want the bus and devices send Bus Request (BR) to the arbiter.  
2) The central arbiter gives the grant → the arbiter sends the BG signal to the first device in the chain.  
3) The grant signal passes serially → BG moves Device1 → Device2 → Device3 …  
4) Each device checks its request.  
5) The first requesting device accepts the grant, and as soon as a device accepts BG, it blocks the signal and it does not go to the next devices.  
6) The selected device uses the bus and performs data transfer.  
7) The bus is released and after transfer the chain becomes free.  

###### Priority Rule

The device closest to the arbiter → Highest Priority  
The last device in the chain → Lowest Priority  

---

##### ii) Parallel Priority

In this method, each device has a separate request line and the arbiter checks all requests at the same time and grants the bus to the highest priority device. In parallel arbitration, priority is fixed by hardware wiring and priority encoder logic during system design.

###### Mechanism

• Multiple devices want the bus  
→ Each device asserts its Bus Request line (BR1, BR2, BR3 …).  

• Requests reach the arbiter in parallel  
→ The arbiter reads all signals at the same time.  

• Priority logic is applied  
→ The arbiter uses a fixed priority encoder  
(e.g., Device1 > Device2 > Device3)  

• Highest-priority device is selected  
→ The arbiter gives Bus Grant (BG) to that device.  

• Selected device uses the bus  
→ It completes data transfer.  

• Bus is released  
→ The next request is processed.  

---

##### iii) Rotating Priority

In this method, priority changes after every bus grant so that all devices get a fair chance and starvation does not occur. In simple terms, the device that gets the bus now becomes the lowest priority next time. In rotating priority arbitration, the first device is selected based on an initial priority order set during system reset, after which priority rotates dynamically.

###### Mechanism

• Initial priority order is set  
→ Example start: D1 > D2 > D3 > D4  

• Multiple devices request the bus  
→ All assert their BR lines.  

• Arbiter selects the highest-priority active request  
→ In the first cycle, D1 is selected.  

• Bus grant is given to D1  
→ D1 completes data transfer.  

• Priority rotates  
→ D1 becomes the lowest.  
New order: D2 > D3 > D4 > D1  

• Next arbitration cycle starts  
→ Now the highest active device at the top is selected.  

• Process continuously repeats  
→ After every grant, priority keeps rotating.  

---

### 2) Distributed Bus Arbitration

In this method, there is no central arbiter; instead, every device has its own decision logic and devices exchange signals among themselves to decide who will take control of the bus.

#### Main Core Components

i) **Request Line (BR)** – It is a communication path where each device sends its bus access demand so that all devices know who wants to use the bus.  

ii) **Priority Logic** – It is the mechanism that decides, if more than one device sends a request, which one will access the bus first.  

iii) **Grant Line (BG)** – Signals are sent on this line to officially give bus access to the winning device so that it can start data transfer.  

---

#### Working Mechanism

i) **Request broadcast** – When a device wants to use the bus, it broadcasts its request signal on the bus. This request is visible to all devices.  

ii) **Local observation** – Each device observes all requests on the bus using its local priority logic. There is no central controller; each device monitors independently.  

iii) **Priority comparison** – Each device compares its priority with other devices’ requests. If it detects a higher priority request, it withdraws its own request.  

iv) **Winner selection** – The device with the highest priority detects that there is no higher priority request against it.  

v) **Grant assertion** – The winning device asserts its grant line and occupies the bus.  

vi) **Bus usage** – The device performs data transfer, and when the work is completed, it frees the bus.  

---

#### Priority Algorithms

##### i) Self-Selection

It is a distributed bus arbitration method in which there is no central arbiter; instead, each device sends its unique priority number (ID) on the bus. The device with the highest number wins the bus.

###### Working Mechanism

1. Each device is assigned a unique binary priority number.  
2. When multiple devices request the bus, all send their priority numbers simultaneously on the bus lines.  
3. Bus lines work on the wired logic principle.  
4. Each device continuously monitors the bits appearing on the bus.  
5. If at any bit position a device has 0 and detects 1 on the bus, it understands that a higher priority device has made a request.  
6. In this condition, the device immediately withdraws from the competition.  
7. The device that does not withdraw till the end takes control of the bus.  

---

##### ii) Collision Detection

It is a distributed bus arbitration technique in which if multiple devices send data on the bus at the same time, collision is detected. The system immediately identifies that a data conflict has occurred, and devices must stop transmission and try again later. There is no fixed priority winner decided in advance.

###### Working Mechanism

1. When a device wants to transmit data, it first senses whether the bus is free or not.  
2. If the bus is idle, the device starts transmission.  
3. If more than one device transmits at the same time, signals overlap and collision occurs.  
4. Each transmitting device continuously monitors the bus and compares its sent signal with the actual bus signal.  
5. If a mismatch is detected, the device understands that a collision has occurred.  
6. As soon as collision is detected, devices immediately stop transmission.  
7. Devices wait for a random backoff time.  
8. After the waiting time, devices again attempt to access the bus.  


## 20. Advantages and Limitations

### Advantages:
* **Simple Design:** Shared paths make the architecture easy to design.
* **Low Cost:** Using a single bus reduces wires and components.
* **Easy Expansion:** New devices can be added easily without major changes.
* **Device Independence:** Different types of devices can communicate over the same bus.
* **Scalable:** Can be scaled from small to large systems.

### Limitations:
* **Limited Bandwidth:** Shared buses mean the total capacity is divided among all devices.
* **Bus Contention:** Only one device can use the bus at a time, leading to delays and conflicts.
* **Speed Limitation:** A slow bus can restrict the performance of a fast CPU and memory.
* **Scalability Issues:** Adding too many devices increases signal noise and timing problems.
* **Bottleneck:** The overall system speed is reduced if the bus cannot keep up with the CPU and memory.
