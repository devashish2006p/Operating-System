# 1. Segmentation
Segmentation ek memory-management technique hai jisme process ke address space ko alag-alag logical parts, yani segments, mein divide kiya jata hai—jaise Code, Heap aur Stack. Har segment ka apna Base aur Bounds register hota hai, jisse OS segments ko physical memory mein alag-alag jagah rakh sakta hai.

# 2. Memory Organization
## 2.1 Physical Memory Organization
Segmentation physical RAM ko kisi special structure mein organize nahi karti. Physical memory khud basically ek linear array of physical addresses hoti hai. Segmentation ka effect ye hota hai ki OS us RAM mein variable-size contiguous regions allocate karke unhe different logical segments ke saath associate karta hai.

## 2.2 Virtual Memory Organization
Virtual memory mein process ko ek single continuous usable region ki tarah nahi, balki multiple logical segments ki tarah organize kiya jata hai.
