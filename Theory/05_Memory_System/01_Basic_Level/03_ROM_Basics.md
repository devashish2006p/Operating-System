# 1. Read Only Memory (ROM)
ROM (Read Only Memory) is a **non-volatile computer memory**.  
This means data stored in ROM remains even after the power is turned off.  
It is mainly used to store permanent instructions or firmware that help the system boot and run.

---

# 2. Purpose of ROM
- Stores **booting instructions** for starting the computer.  
- Handles **hardware initialization** during startup.  
- Keeps **permanent settings** in embedded devices.  
- Stores **programs in microcontrollers**.  
- Holds **game code in cartridges** for consoles.  

---

# 3. Types of ROM

## 1. MROM (Masked ROM)
Data is **hard-wired during manufacturing**.  
User cannot change or reprogram it.  
Used for fixed permanent instructions.

---

## 2. PROM (Programmable ROM)
Initially blank memory.  
User can program it **once** by burning fuse connections.  
Data cannot be erased or changed after programming.

---

## 3. EPROM (Erasable Programmable ROM)
Data can be erased using **UV light** through a quartz window on the chip.  
After erasing, it can be reprogrammed multiple times.  
Used in development and testing stages.

---

## 4. EEPROM (Electrically Erasable Programmable ROM)
Data can be erased and rewritten using **electrical signals**.  
Supports **byte-by-byte erase/write**, making it flexible.  
Used in devices where small updates are needed (BIOS, embedded systems).

---

## 5. Flash Memory
Advanced form of EEPROM.  
Data is erased/written in **blocks instead of bytes**, making it faster and efficient.  
Suitable for **large storage** like USB drives, SSDs, and memory cards.

---

# 3. Characteristics of ROM
- **Non-volatile:** Data stays even without power.  
- **Permanent storage:** Ideal for firmware and boot instructions.  
- **Different programmability:** Some ROM types are fixed, others can be reprogrammed.  
- **Reliability:** Ensures system always has startup instructions.  

---

# 4. Applications of ROM
- **Computer BIOS/UEFI firmware.**  
- **Embedded devices** (washing machines, microwaves).  
- **Microcontrollers** for industrial and consumer electronics.  
- **Game cartridges** for consoles.  
- **Storage devices** (USB, SSD, memory cards using Flash).  

---

# 5. Key Differences Between ROM Types

| Type    | Programmability         | Erase Method        | Usage Example              |
|---------|-------------------------|---------------------|----------------------------|
| MROM    | Fixed at manufacturing  | Not erasable        | Permanent boot code        |
| PROM    | Programmed once         | Not erasable        | Device configuration       |
| EPROM   | Reprogrammable          | UV light erase      | Development/testing chips  |
| EEPROM  | Reprogrammable          | Electrical signals  | BIOS, embedded devices     |
| Flash   | Reprogrammable          | Block electrical erase | SSDs, USB drives, memory cards |

---

# 6. ROM Packaging Types
Packaging type means the **physical form in which ROM chips are arranged and connected** to a device.  
It defines the size, pin layout, and how ROM fits into motherboards or embedded systems.

- **DIP (Dual Inline Package):** Small rectangular chip with pins on both sides, used in early computers and microcontrollers.  
- **SIMM (Single Inline Memory Module):** Early module form, now obsolete, used in older PCs.  
- **DIMM/SO-DIMM style modules:** Modern packaging for Flash-based ROM, similar to RAM sticks, used in desktops and laptops.  
- **Embedded soldered chips:** ROM directly soldered onto circuit boards in microcontrollers, consoles, and consumer electronics.  

---

# 7. ROM Generations (Evolution)
ROM evolved step by step to allow more flexibility and reusability.  
Each generation improved programmability, erase methods, and storage efficiency.

- **MROM (Masked ROM):** Hard-wired during manufacturing, fixed data only.  
- **PROM (Programmable ROM):** User can program once, but cannot erase or change.  
- **EPROM (Erasable PROM):** Can be erased with UV light and reprogrammed multiple times.  
- **EEPROM (Electrically Erasable PROM):** Erased and rewritten using electrical signals, byte-level control.  
- **Flash Memory:** Advanced EEPROM, erases/writes data in blocks, faster and suitable for large storage.  
- **NOR Flash:** Allows fast random access, used in firmware storage.  
- **NAND Flash:** High density, optimized for bulk storage like SSDs and memory cards.  

---

# 8. ROM Hierarchy in Computer Systems
ROM exists at different levels in computing devices, each serving a specific role.  
Hierarchy shows how ROM supports both system startup and permanent storage.

- **Firmware ROM (BIOS/UEFI):** Stores boot instructions and hardware initialization code in PCs.  
- **Embedded ROM:** Found in microcontrollers and consumer electronics (washing machines, remotes, cars).  
- **Game/Console ROM:** Permanent game code stored in cartridges or embedded chips.  
- **Flash Storage ROM:** Large-scale non-volatile memory used in SSDs, USB drives, and memory cards.  

---
