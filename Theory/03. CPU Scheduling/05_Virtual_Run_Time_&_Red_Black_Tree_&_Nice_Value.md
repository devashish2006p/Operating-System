## Virtual Runtime (vruntime)

**Definition:**
Virtual Runtime is a *logical time* used by the Linux scheduler to measure how much CPU time a process has received **in a fair and priority‑weighted manner**.

**Simple Meaning:**
It tells the scheduler **which process has used the CPU more and which has used it less**, after considering priority.

**Key Points:**

* Not real clock time
* Calculated logically by the scheduler
* Based on CPU usage + process priority
* Main base for scheduling decisions in CFS (Completely Fair Scheduler)

**Important for Exam:**
Process with **lowest vruntime** gets CPU first

---

## Red‑Black Tree

**Definition:**
A Red‑Black Tree is a **self‑balancing binary search tree** used by the Linux scheduler to store runnable processes efficiently.

**Simple Meaning:**
It is a special tree structure that keeps itself balanced automatically so that searching the next process is fast.

**Important Notes:**

* Every node has a **color**: Red or Black
* Color has **no physical meaning**
* Colors are used only to maintain balance

**Why Linux Uses It:**

* Fast insertion
* Fast deletion
* Fast lookup of next process

Time complexity: **O(log n)**

---

## Rules of Red‑Black Tree

1. Every node is either **Red** or **Black**
2. The **root node is always Black**
3. A **Red node cannot have a Red child**
4. Every path from root to leaf has the **same number of Black nodes**
5. All **leaf (NULL) nodes are Black**

**Exam Tip:**
These rules ensure the tree always remains balanced

---

## Nice Value

**Definition:**
Nice value is a **process attribute** in Linux that controls the **priority of a process**.

**Range:**

* **-20** → Highest priority (less nice)
* **+19** → Lowest priority (more nice)
* **0** → Default value

**Simple Meaning:**
Nice value tells the scheduler **how nice a process is to others** when sharing CPU time.

**Important Points:**

* Lower nice value → more CPU time
* Higher nice value → less CPU time
* Affects virtual runtime calculation

---

## Relation Between All Three

* **Nice value** decides process priority
* Priority affects **Virtual Runtime**
* Processes are stored in **Red‑Black Tree**
* Scheduler picks process with **minimum vruntime**

---

