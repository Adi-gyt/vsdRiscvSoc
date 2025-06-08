## ✅ Task 13: Interrupt Primer (RISC-V Bare-Metal)

### 🎯 Objective
Enable the machine-timer interrupt (MTIP) and write a simple handler in C for a bare-metal RISC-V system.

### 🧩 Concept Overview

This task demonstrates how to:
- Use the RISC-V **machine timer (mtime / mtimecmp)** to generate interrupts.
- Write an **interrupt handler** in C using `__attribute__((interrupt))`.
- Use **memory-mapped I/O** to simulate UART output.
- Work with **CSRs (Control and Status Registers)** for enabling interrupts.

### 🧠 Key Concepts Learned

#### ✅ Timer Interrupts
- The **mtime** register keeps incrementing.
- When **mtime ≥ mtimecmp**, the **MTIP** bit triggers an interrupt.

#### ✅ Interrupt Handling
- Wrote a C handler for the timer interrupt:
```c
void __attribute__((interrupt)) timer_handler() {
    volatile uint32_t *uart = (uint32_t*)0x10000000;
    *uart = 'I';
    *(volatile uint64_t*)MTIMECMP += 100000;
}
```
✅ CSRs Used
mie → Enables machine-level interrupts (MTIE bit).

mstatus → Enables global interrupts (MIE bit).

✅ Memory-Mapped I/O
UART at address 0x10000000 is used to output characters to the terminal (QEMU simulation).

🛠️ Code and Compilation
🔹 interrupt.c
```c

#include <stdint.h>

#define MTIME       0x200BFF8
#define MTIMECMP    0x2004000

void __attribute__((interrupt)) timer_handler() {
    volatile uint32_t *uart = (uint32_t*)0x10000000;
    *uart = 'I';
    *(volatile uint64_t*)MTIMECMP += 100000;
}

void _start() {
    volatile uint32_t *uart = (uint32_t*)0x10000000;
    *uart = 'S';

    *(volatile uint64_t*)MTIMECMP = *(volatile uint64_t*)MTIME + 100000;

    asm volatile (
        "li t0, 0x88\n"       // MIE_MTIE (0x80) + MSTATUS_MIE (0x8)
        "csrs mie, t0\n"
        "csrs mstatus, t0\n"
    );

    while(1);
}
```
🔹 Compile
```bash
riscv32-unknown-elf-gcc -march=rv32imc_zicsr -mabi=ilp32 -nostdlib -ffreestanding \
  -Wl,-Ttext=0x80000000 -e _start -o interrupt.elf interrupt.c
-march=rv32imc_zicsr: Enables CSR access
-nostdlib: No libc or startup files
-ffreestanding: We’re not assuming a standard environment
```
▶️ Run in QEMU
```bash
qemu-system-riscv32 -nographic -machine virt -bios none -kernel interrupt.elf
```
🧪 Expected Output
```
SIIIIIIIIIIIIIIIIIIII...
```
S → Printed at startup

I → Printed periodically by timer interrupt
![Screenshot 2025-06-08 181642](https://github.com/user-attachments/assets/62062a4f-3ed7-429c-b207-4dc840008e75)

✅ Why This Matters
Interrupts are essential for real-time response in embedded systems.

Bare-metal timer configuration and interrupt handling are foundational for OS development.

Reinforced working with CSRs, memory-mapped I/O, and toolchain flags.
