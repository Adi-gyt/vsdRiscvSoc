## ✅ Task 15: Atomic Test Program (Using `lr.w` and `sc.w`)
### 🎯 Objective
Use atomic RISC-V instructions (`lr.w` and `sc.w`) to safely increment a shared variable using a spinlock.
### 🧾 Step-by-Step Execution

#### 📝 Step 1: Code – `atomic.c`
```c
volatile int lock = 0;
volatile int shared_counter = 0;

void acquire_lock() {
    int tmp;
    do {
        asm volatile (
            "lr.w %0, (%1)\n"
            "sc.w %0, %2, (%1)\n"
            : "=&r"(tmp)
            : "r"(&lock), "r"(1)
            : "memory");
    } while (tmp);
}

void release_lock() {
    lock = 0;
}

void _start() {
    for (int i = 0; i < 5; i++) {
        acquire_lock();
        shared_counter++;
        release_lock();
    }
    while (1);
}
```
✅ This code demonstrates a mutex-style spinlock using RISC-V’s atomic lr.w (load-reserved) and sc.w (store-conditional).

🔧 Step 2: Linker Script
Ensure the linker script (e.g., link.ld) sets the load address to something like 0x80200000, consistent with OpenSBI expectations.

🧪 Step 3: Compile
```
bash
riscv64-unknown-elf-gcc -nostartfiles -nostdlib -march=rv32imac -mabi=ilp32 \
  -T link.ld -o task15.elf atomic.c syscalls.c
```
-nostdlib -nostartfiles: Bare-metal compilation
syscalls.c: Custom stub for environment needs (minimal, empty versions acceptable)

▶️ Step 4: Run in QEMU
```bash
qemu-system-riscv32 -nographic -machine virt \
  -bios /media/sf_firmware/fw_jump.bin \
  -kernel task15.elf \
  -s -S
```
-s -S: Waits for debugger connection (port 1234)

🐞 Step 5: Inspect in GDB
In a second terminal:

```bash
riscv64-unknown-elf-gdb task15.elf
(gdb) target remote :1234
(gdb) break _start
(gdb) continue
(gdb) print shared_counter
```
✅ Expected Output:

```gdb
$1 = 5
```
This confirms the shared counter incremented atomically in a safe region guarded by acquire_lock() and release_lock().

📚 What I Learned
Atomic Instructions:

lr.w: Performs a load and reserves the memory address

sc.w: Stores only if the reservation is still valid

Together, they form a lock-free synchronization primitive

Spinlocks: Used lr.w/sc.w loop to ensure mutual exclusion

Bare-Metal Debugging:

Ran and paused execution in QEMU with -s -S

Used GDB to inspect memory/registers

🧠 Why This Matters
⚙️ Multithreading and Concurrency:

Essential for writing OS kernels, real-time applications, and drivers

🔐 Synchronization:

Understanding atomic primitives helps implement critical sections

🔍 Debugging RISC-V Hardware Code:

Valuable skill to inspect memory in QEMU using GDB
