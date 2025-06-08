# ✅ Task 9: Inline Assembly – Reading the cycle CSR 🧠

## 🎯 Task Goal

Use RISC-V inline assembly to read the `cycle` CSR register and print the difference in CPU cycles between two code points using UART.

## 📄 Source: `task9_cycle.c`
```c
#include <stdint.h>

#define UART ((volatile char *)0x10000000)

static inline uint32_t rdcycle(void) {
    uint32_t c;
    asm volatile ("csrr %0, cycle" : "=r"(c));
    return c;
}

void print_hex(uint32_t val) {
    const char hex[] = "0123456789ABCDEF";
    for (int i = 7; i >= 0; i--) {
        UART[0] = hex[(val >> (i * 4)) & 0xF];
    }
    UART[0] = '\n';
}

void _start() {
    print_hex(0xDEADBEEF);  // UART check

    uint32_t before = rdcycle();
    for (volatile int i = 0; i < 100000; i++);
    uint32_t after = rdcycle();

    print_hex(after - before);

    while (1);  // Halt
}
```
🔧 link.ld
```ld

ENTRY(_start)

SECTIONS {
  . = 0x80400000;

  .text : { *(.text*) }
  .rodata : { *(.rodata*) }
  .data : { *(.data*) }
  .bss : {
    *(.bss*)
    *(COMMON)
  }
}
```
🛠️ Makefile
```make

CC = riscv64-unknown-elf-gcc
CFLAGS = -march=rv32imac_zicsr -mabi=ilp32 -nostartfiles -nostdlib -Wall

all: task9.elf

task9.elf: task9_cycle.c link.ld
	$(CC) $(CFLAGS) -o task9.elf task9_cycle.c -T link.ld

clean:
	rm -f task9.elf
```
▶️ How I Ran It
🧪 Steps:
```bash
make clean
make
qemu-system-riscv32 -nographic -machine virt \
  -bios /media/sf_firmware/fw_jump.bin \
  -kernel task9.elf
```
✅ Expected Output:
```
DEADBEEF
00003ACF
```
DEADBEEF confirms UART is printing correctly.

Second line shows the cycle difference between rdcycle() calls.

📘 What I Learned
Used inline assembly in C to access Control and Status Registers (CSRs).

Learned how asm volatile ("csrr %0, cycle" : "=r"(c)); works:

csrr reads the current cycle count.

%0 maps the result into a C variable (c).

volatile prevents reordering/removal by compiler.

Gained deeper understanding of bare-metal cycle timing.

Practiced hex printing to UART and basic benchmarking.
