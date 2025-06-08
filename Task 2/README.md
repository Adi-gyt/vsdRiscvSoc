# ✅ Task 2: Compile “Hello, RISC-V”
## 📝 Task Description
The objective of this task was to write a minimal C program that prints `"Hello, RISC-V"` and cross-compile it using the **RISC-V GCC toolchain** targeting the `rv32imc` architecture. The goal was to produce a valid RISC-V ELF executable and verify the architecture and format.
### 📄 Create a Minimal C Program
File: `hello.c`
```c
#include <stdio.h>

int main() {
    printf("Hello, RISC-V\n");
    return 0;
}
```
🛠️ Cross-Compiled with RISC-V GCC
I compiled the program using the following command:

```bash
riscv32-unknown-elf-gcc -march=rv32imc -mabi=ilp32 -o hello.elf hello.c
```
✅ This produced an ELF binary named hello.elf.

📦 Verified the ELF Output
To verify the compiled file format, I used:

```bash
file hello.elf
```
📌 Output:![hello](https://github.com/user-attachments/assets/2652b7ff-964a-4d39-b5d3-82a36e71dc56)

```
hello.elf: ELF 32-bit LSB executable, UCB RISC-V, version 1 (SYSV), statically linked, not stripped
```
📚 What I Learned
Gained hands-on experience compiling a C program using the riscv32-unknown-elf-gcc toolchain.

Learned the significance of:

-march=rv32imc — targets the RV32 ISA with Integer, Multiplication, and Compressed instructions.

-mabi=ilp32 — sets the Integer calling convention and memory layout.

Understood the basics of ELF file formats and used the file command to inspect compiled binaries.

