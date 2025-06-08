# ✅ Task 11: Linker Script 101 – Placing Code and Data in Custom Memory Regions
## 🎯 Objective

Create a custom linker script that:
- Places `.text` (code) section at address `0x00000000` (Flash)
- Places `.data` section at address `0x10000000` (RAM)

This mirrors a typical embedded system layout: **Flash for instructions, RAM for data**.

## 🧱 Step 1: Linker Script – `task11.ld`

```ld
ENTRY(_start)

SECTIONS {
  /* Code goes into Flash (0x00000000) */
  . = 0x00000000;

  .text : {
    *(.text.entry)
    *(.text*)
  }

  /* Data goes into RAM (0x10000000) */
  .data : {
    . = 0x10000000;
    *(.data*)
  }

  /* Uninitialized data (BSS) also goes into RAM */
  .bss : {
    *(.bss*)
    *(COMMON)
  }
}
```
📄 Step 2: Test Program – task11_test.c
```c

#include <stdint.h>

volatile int my_data = 42;

void _start() {
    my_data = 99;
    while (1);  // Halt
}
my_data resides in the .data section.

_start() modifies it, ensuring it’s treated as a live variable in RAM.
```
🛠️ Step 3: Compilation
```bash
riscv32-unknown-elf-gcc -nostartfiles -T task11.ld \
  -o task11.elf task11_test.c
```
🧪 Step 4: Inspect Section Placement
```bash
riscv32-unknown-elf-objdump -h task11.elf
```
✅ Example Output:

```
Idx Name      Size     VMA          LMA          File off  Algn
 0 .text      000000XX 00000000     00000000     ...
 1 .data      00000004 10000000     10000000     ...
```
![task11](https://github.com/user-attachments/assets/c65e349b-34c6-4862-adfd-cf8a65102156)

.text placed at 0x00000000 ✅

.data placed at 0x10000000 ✅

This confirms the linker script correctly assigned sections.

📘 What I Learned
How to manually control section placement using a linker script.

The difference between Flash (code storage) and RAM (data storage) in embedded systems.

How to use objdump to verify memory layouts of compiled binaries.

Practical experience building a bare-metal memory map for RISC-V.
