# ✅ Task 10: Memory-Mapped I/O – GPIO Toggle Demo
## 🎯 Task Goal
Access a memory-mapped GPIO register (`0x10012000`) and toggle its value to simulate turning an LED on and off in a bare-metal environment using C.
## 🧱 What is Memory-Mapped I/O?
Memory-mapped I/O allows software to control hardware by reading from or writing to specific memory addresses. In this case:
```c
#define GPIO (*(volatile uint32_t*)0x10012000)
```
This line creates a volatile pointer to a memory location mapped to GPIO. Using volatile ensures the compiler doesn't optimize away reads/writes to this register.

📄 Source Code: task10_gpio.c
```c
#include <stdint.h>

#define GPIO (*(volatile uint32_t*)0x10012000)

void _start() {
    GPIO = 0x1;  // Turn ON (e.g., LED ON or pin HIGH)

    for (volatile int i = 0; i < 100000; i++);  // Simple delay loop

    GPIO = 0x0;  // Turn OFF (e.g., LED OFF or pin LOW)

    while (1);  // Halt the CPU
}
```
📄 Linker Script: link.ld
```ld
ENTRY(_start)

SECTIONS {
  . = 0x80200000;

  .text : { *(.text.entry) *(.text*) }
  .data : { *(.data*) }
  .bss  : { *(.bss*) *(COMMON) }
}
```
🛠️ Compilation
```bash
riscv32-unknown-elf-gcc -march=rv32imac -mabi=ilp32 -nostartfiles -T link.ld \
  -o task10_gpio.elf task10_gpio.c
```
▶️ Running the Program
```bash
qemu-system-riscv32 -nographic -machine virt \
  -bios /media/sf_firmware/fw_jump.bin \
  -kernel task10_gpio.elf
```
✅ Expected Result
There’s no UART output, but QEMU should keep running silently.

No crash or segmentation fault means the program executed successfully.

On real hardware or with GDB, you could inspect address 0x10012000 to confirm the writes.

📘 What I Learned
How to directly access and manipulate hardware using memory-mapped I/O in a RISC-V bare-metal setup.

How to set up a custom linker script to control memory layout.

Why volatile is essential for accessing peripheral registers.

Practiced writing minimal C startup code without main() or an operating system.
