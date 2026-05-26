# 1. Definition 
Memory Address RAM ke andar kisi specific memory location ka unique numeric identifier hota hai jiske through CPU data ko read ya write karta hai.

# 2. Address Space 
Address space ka matlab hai system ya process ke liye available sabhi possible memory addresses ka complete range, jinhe use karke CPU memory ko access kar sakta hai.

## 2.1 Types of Address Space 

## 2.1.1 Virtual Address Space 
Virtual address space wo memory address range hota hai jo OS ek process ko provide karta hai jisme process memory ko access karta hai, chahe actual physical RAM kahi aur map ho.

## 2.1.2 Physical Address Space 
Physical address space wo sabhi actual memory addresses ka range hota hai jo real RAM hardware me exist karta hai aur jise system directly access kar sakta hai.

# 3. Address Range, Size, Width
1. Address Range - Address range ka matlab hai lowest address se highest address tak ka interval jisme memory addresses exist karte hain. Address range nikalne ka lia 2^address width - 1 kar dete hai. 
2. Address Size - Address size ka matlab hai total number of memory locations jo address space me exist karte hain. Address size depend karta hai address width par. Address size nikalne ka lia 2^address width kar dete hai. 
3. Address Width - Address width ka matlab hai kitne bits use ho rahe hain ek memory address ko represent karne ke liye. Address width mainly CPU architecture par depend karta hai. Agr CPU architecutre 16 bits ka hai to address width v 16 bits ka he hoga, agr 32 hai to 32 bits aur 64 bits hai to 64 bits ka hota hai address width. 

# 4. Addressable Unit/Granularity
Addressable unit/Granularity wo smallest amount of data hota hai jo ek memory address point karta hai aur jise CPU directly access kar sakta hai.
- There are three types of addressable unit:-
  1. Bit Addressable - Ishme har address 1 bit ko point karta hai. 
  2. Byte Addressable - Ishme har address 1 byte ko point karta hai. 
  3. Word Addressable - Ishme har address CPU ke word size (16/32/64 bits) ko point karta hai.

# 5. Endianness 
Ya ek rule hai jo btata hai ki multi byte data ka bytes memory ma kis order ma store honge. Ya decide karta hai ki Most Significant Byte (MSB) pehle store hoga ya Least Significant Byte (LSB) pehle store hoga. 
## 5.1 Types of Endianness 
1. Little Endian - Little Endian me multi-byte data ka Least Significant Byte (LSB) sabse pehle (lowest memory address par) store hota hai.

2. Big Endian - Big Endian me multi-byte data ka Most Significant Byte (MSB) sabse pehle (lowest memory address par) store hota hai.
   
 
 
