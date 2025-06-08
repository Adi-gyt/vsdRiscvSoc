## ✅ Task 16: printf() Without Standard Library (Bare-Metal RISC-V)
### 🎯 Objective
Use `printf()`-style functionality in a bare-metal RISC-V environment **without linking libc or stdio.h**, by implementing a custom `_write()` system call that outputs directly to UART.
### 🧾 Code Walkthrough
#### 🗂️ main.c
```c
extern int _write(int, const char*, int);  // Declare custom _write

int strlen(const char *s) {
    int len = 0;
    while (s[len] != '\0') len++;
    return len;
}

void print(const char *s) {
    _write(1, s, strlen(s));  // Write to UART
}

void _start() {
    print("Hello from RISC-V!\n");
    while (1);
}
```
🛠️ syscalls.c
```c
#include <stdint.h>

#define UART_TX (*(volatile uint32_t*)0x10000000)

int _write(int fd, const char *buf, int len) {
    for (int i = 0; i < len; i++) {
        UART_TX = buf[i];  // Direct memory-mapped I/O
    }
    return len;
}
```
⚙️ Compilation
```bash
riscv64-unknown-elf-gcc -Wall -O0 -ffreestanding -nostartfiles -nostdlib \
  -march=rv32imac -mabi=ilp32 -T link.ld \
  main.c syscalls.c -o task16.elf
```
✅ Flags used:

-nostdlib -nostartfiles: No standard startup code or libraries

-ffreestanding: For bare-metal programming

-T link.ld: Ensure correct load address

▶️ Execution in QEMU
```bash
qemu-system-riscv32 -nographic -machine virt \
  -bios /media/sf_firmware/fw_jump.bin \
  -kernel task16.elf \
  -serial file:uart.log
```
🔁 UART output redirected to uart.log

📤 Terminal Output
After execution, you can view the result with:

```bash
cat uart.log
```
Expected content:

```csharp
Hello from RISC-V!

```
![Screenshot from 2025-06-07 13-52-47](https://github.com/user-attachments/assets/d02c3dd6-5de3-472f-8a90-ae460e2a75f4)

🧠 What I Learned
✅ How printf() depends only on _write() under the hood

✅ Implemented a minimal print() function using only custom code

✅ Direct hardware I/O using memory-mapped UART

✅ UART redirection in QEMU using -serial file:filename
