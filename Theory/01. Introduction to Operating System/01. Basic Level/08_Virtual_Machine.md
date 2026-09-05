# 1. Virtual Machine 
Virtual Machine ek software-created computer hota hai jo tumhare real/physical computer ke andar ek alag computer ki tarah run karta hai.

Matlab tumhare paas ek physical computer hai, lekin software ki help se uske andar tum ek ya multiple virtual computers bana sakte ho.

# 2. Types of Virtual Machine
1. System Virtual Machine - System Virtual Machine ek software ke through banaya gaya virtual computer environment hota hai jo ek physical computer ke resources ka use karke poore operating system ko aise run karta hai jaise woh ek alag physical computer ho.
   
2. Process Virtual Machine - Process Virtual Machine ek software-based virtual environment hota hai jo sirf ek particular program ko run karne ke liye zaroori environment provide karta hai, bina poora operating system virtualize kiye.

# 3. Hypervisor 
Hypervisor ek software ya software layer hota hai jo physical computer ke hardware resources ko manage karke Virtual Machines ko virtual CPU, RAM, storage aur other hardware resources provide karta hai.

- **Types of Hypervisor**
1. Bare-Metal Hypervisor : Ya directly physical hardware ka upar run karta hai aur ushke upar virtual machines chalti hai. 

2. Hosted Hypervisor : Ya pehle sa installed Host Operating System ka uper application/software ki tarah run karta hai aur ushke ander virtual machines chalti hai.

- **How Hypervisor Works**
1. Sabse pehle physical computer/server mein actual hardware hota hai, jaise CPU, RAM, storage aur network devices.
2. Hypervisor physical hardware par run hota hai; Type 1 mein directly hardware par aur Type 2 mein Host OS ke upar run hota hai.
3. Hypervisor physical hardware ke CPU, RAM, storage aur network resources ko control aur manage karta hai.
4. Jab administrator ek Virtual Machine create karta hai, to Hypervisor us VM ko kuch physical resources allocate karta hai, jaise 2 CPU cores, 4 GB RAM aur 50 GB storage.
5. Hypervisor in physical resources ko VM ke saamne virtual hardware ke form mein present karta hai, jaise Virtual CPU, Virtual RAM, Virtual Disk aur Virtual Network Card.
6. Uske baad VM ke andar Guest Operating System install kiya jata hai, jaise Windows, Ubuntu ya Kali Linux.
7. Guest OS ko virtual hardware ek normal computer ke hardware ki tarah dikhai deta hai, isliye Guest OS apne normal processes aur applications ko run kar sakta hai.
8. Jab Guest OS CPU, RAM, storage ya network ko use karna chahta hai, to uski request Hypervisor ke resource-management mechanism se handle hoti hai.
9. Hypervisor Guest OS ke virtual resources ko actual physical resources se connect karta hai, jaise Virtual CPU ko physical CPU par aur virtual memory ko physical RAM par map karta hai.
10. Agar ek hi physical machine par multiple VMs chal rahi hain, to Hypervisor un sabke resources ko manage karta hai taaki ek VM ke resources doosri VM ko unnecessarily affect na karein.
11. Jab VM ko koi kaam karna hota hai, Hypervisor required physical resources provide karta hai aur underlying hardware par us kaam ko execute karwata hai.
12. Jab VM band hoti hai, Hypervisor us VM ke allocated resources ko release karke doosri VMs ya system ke liye available kar deta hai.

- **How VM Works**
1. Sabse pehle physical computer ke paas actual hardware hota hai, jaise CPU, RAM, storage aur network device.
2. Hypervisor physical hardware ke upar kaam karta hai aur hardware ke resources ko manage karta hai.
3. Hypervisor physical resources ka ek portion Virtual Machine ko allocate karta hai, jaise 2 CPU cores, 4 GB RAM aur 50 GB virtual storage.
4. Hypervisor in resources ko VM ke liye virtual hardware ke form mein present karta hai, jaise virtual CPU, virtual RAM, virtual disk aur virtual network card.
5. VM ke andar Guest Operating System install kiya jata hai, jaise Ubuntu, Kali Linux ya Windows.
6. Guest Operating System ko lagta hai ki uske paas ek complete computer hai, isliye woh apne normal processes, files aur applications ko run karta hai.
7. Jab Guest OS CPU par koi instruction execute karna chahta hai, to virtualization mechanism us instruction ko physical CPU par execute karwata hai.
8. Jab Guest OS memory use karta hai, to uski virtual memory addresses ko underlying physical memory se map kiya jata hai.
9. Jab Guest OS virtual disk par data save karta hai, to hypervisor us data ko physical storage par available virtual-disk representation mein store karwata hai.
10. Jab Guest OS network access karta hai, to uska virtual network card hypervisor ke through physical network hardware se communicate karta hai.
11. Is tarah Guest OS aur uske applications apna kaam normally karte hain, jabki background mein hypervisor unke virtual resources ko physical hardware ke resources se connect aur manage karta rehta hai.
12. End result ye hota hai ki ek physical computer ke andar ek complete separate computer environment ki tarah VM operate karti hai, bina physically alag computer banaye.
