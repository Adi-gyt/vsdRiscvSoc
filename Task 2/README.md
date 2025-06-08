# ✅ Task 2: Compile “Hello, RISC-V”
## 📝 Task Description
The objective of this task was to write a minimal C program that prints `"Hello, RISC-V"` and cross-compile it using the **RISC-V GCC toolchain** targeting the `rv32imc` architecture. The goal was to produce a valid RISC-V ELF executable and verify the architecture and format.
### 📄 Create a Minimal C Program
Create hello.c using a text editor (e.g., nano):
```bash
nano hello.c
```
```c
#include <stdio.h>

int main() {
    printf("Hello, RISC-V\n");
    return 0;
}
```
🛠️ Cross-Compiled with RISC-V GCC
Run this command to cross-compile:

```bash
riscv32-unknown-elf-gcc -march=rv32imc -mabi=ilp32 -o hello.elf hello.c
```
Flags:

-march=rv32imc: Target architecture (RV32IMC = Base + Multiply + Compressed).

-mabi=ilp32: Application Binary Interface (32-bit integers, longs, pointers).

-o hello.elf: Output filename.

I compiled the program using the following command:

✅ This produced an ELF binary named hello.elf.

📦 Verified the ELF Output
To verify the compiled file format, I used:

```bash
file hello.elf
```
 Verify the ELF File
Check the file type:

```bash
file hello.elf
```
Expected Output:

```
hello.elf: ELF 32-bit LSB executable, UCB RISC-V, version 1 (SYSV), statically linked, not stripped
```
To see the output:

```bash
qemu-system-riscv32 -nographic -machine virt -kernel hello.elf
```
Expected Output:

plaintext
Hello, RISC-V!

📌 Output:![hello](https://github.com/user-attachments/assets/2652b7ff-964a-4d39-b5d3-82a36e71dc56)


📚 What I Learned
Gained hands-on experience compiling a C program using the riscv32-unknown-elf-gcc toolchain.

Learned the significance of:

-march=rv32imc — targets the RV32 ISA with Integer, Multiplication, and Compressed instructions.

-mabi=ilp32 — sets the Integer calling convention and memory layout.

Understood the basics of ELF file formats and used the file command to inspect compiled binaries.

