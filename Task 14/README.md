## ✅ Task 14: `rv32imac` vs `rv32imc` — What’s the “A”?

### 🎯 Objective
Understand the role of the ‘A’ (Atomic) extension in the RISC-V instruction set and its difference from `rv32imc`.

---

### 🧠 What Does `rv32imac` Mean?

- `rv32` → 32-bit RISC-V architecture  
- `i` → Integer instructions  
- `m` → Multiply/Divide instructions  
- `a` → **Atomic instructions** ✅  
- `c` → Compressed instructions (16-bit format)

So:
- `rv32imac` includes **atomic operations**
- `rv32imc` does **not** include atomic instructions

---

### 🔑 What Does the “A” Extension Add?

The Atomic (A) extension adds **atomic memory operations** essential for:

- Multithreading / concurrency
- Locking mechanisms (spinlocks, mutexes)
- Operating systems and kernel code
- Shared memory programming

---

### 🔨 Key Instructions from “A” Extension

| Instruction | Description                   |
|-------------|-------------------------------|
| `lr.w`      | Load-Reserved (word)          |
| `sc.w`      | Store-Conditional (word)      |
| `amoswap.w` | Atomic Swap                   |
| `amoadd.w`  | Atomic Add                    |
| `amoor.w`   | Atomic OR                     |
| `amoand.w`  | Atomic AND                    |

---

### 📌 Example: Spinlock with `lr.w` and `sc.w`

```c
volatile int lock = 0;

void lock_acquire() {
    int tmp;
    do {
        asm volatile (
            "lr.w %0, (%1)\n"
            "sc.w %0, %2, (%1)\n"
            : "=&r"(tmp)
            : "r"(&lock), "r"(1)
            : "memory"
        );
    } while (tmp);  // Loop until store succeeds
}

##Important Points

rv32imac includes atomic memory instructions, making it ideal for multithreaded and OS-level development.

rv32imc lacks these, so it’s better suited for simple bare-metal systems.

Atomic instructions allow safe concurrent access to shared memory.


