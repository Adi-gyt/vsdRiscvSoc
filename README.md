# vsdRiscvSoc
# India RISC-V Chip Tapeout – Project-Based Learning
This repository documents my journey through the India RISC-V Chip Tapeout    project-based learning program. The project focuses on setting up and using the  RISC-V Toolchain in a Linux environment, learning about chip design basics, and completing tasks that simulate a hands-on industry-level experience.

Lab Setup Requirements
To begin this project, I followed these steps to set up the required environment:

1. Operating System: Used Oracle VirtualBox to set up a Linux environment.
2. ## ✅ RISC-V GCC Toolchain Setup on Windows (xPack)
```bash
Toolchain:   xPack RISC-V Embedded GCC  
Architecture: riscv32 (supports rv32imac)  
Host OS:     Windows 10/11 (64-bit)
```
## 📥 Installation Steps

### 🔽 Download the Toolchain

Download the Windows 64-bit version of the xPack RISC-V GCC from the official site:  
👉 [xPack RISC-V GCC Releases](https://xpack.github.io/riscv-none-elf-gcc/)
```css
xpack-riscv-none-elf-gcc-12.2.0-1.2-win64.zip
## 🔧 Set Environment Variables
```
Add the `bin` folder inside the toolchain directory to your system `PATH`:

### 🧭 Steps:

1. Open **System Properties** → **Environment Variables**
2. Under **System variables**, find and select the `Path` variable → click **Edit**
3. Click **New** and add the following path:

```makefile
C:\riscv-toolchain\bin
## ✅ Verify Installation
```
After setting up the environment variables, verify the toolchain installation.

### 🔍 Check Toolchain Version

Open `cmd.exe` or `PowerShell`, and run:

```sh
riscv-none-elf-gcc --version
```
Expected Output
![Screenshot 2025-06-08 172121](https://github.com/user-attachments/assets/30b6590c-c09f-46f2-abed-ce5fab5553c4)

```sh
riscv-none-elf-gcc (xPack GNU RISC-V Embedded GCC) 12.2.0
```
Verified that the tool is available in the system path:
```
```sh
where riscv-none-elf-gcc
```

Checked compilation using a sample test program:
```sh
riscv-none-elf-gcc -march=rv32imac -mabi=ilp32 -Wall -ffreestanding -O0 -c hello.c -o hello.o
riscv-none-elf-ld -T linker.ld hello.o -o hello.elf
```
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

# ✅ Task 3: From C to Assembly

## 📝 Task Description
The objective of this task was to convert the `hello.c` program into RISC-V assembly using the RISC-V GCC toolchain, and analyze the resulting assembly code to understand how basic C constructs are translated for the `rv32imc` architecture.

## ⚙️ What I Did

### 🧾 Compiled C to Assembly

Used the following command to generate assembly from `hello.c`:

```bash
riscv64-unknown-elf-gcc -march=rv32imc -mabi=ilp32 -S -O0 hello.c
```
✅ This generated an assembly file: hello.s

🔧 Function Prologue
The beginning of the main function sets up the stack and frame pointer:

```assembly
addi sp, sp, -16     # Allocate stack space
sw   s0, 12(sp)      # Save s0 (callee-saved register)
addi s0, sp, 16      # Establish frame pointer
```
The characters are printed using memory-mapped I/O (e.g., UART). Instructions include:

*lui + lw: Load UART base address

*li: Load character ASCII values

*sw: Send characters to UART

This mimics printing functionality in a bare-metal environment without an OS.

🌀 Program Termination (Infinite Loop)
At the end of the program:

```assembly
.L2:
    j .L2
```
✅ This is an intentional infinite loop to prevent return, as there’s no operating system to return to in a bare-metal environment.

![Task 3](https://github.com/user-attachments/assets/a5e5f2da-d0b6-4b65-998b-097da2f2a5d2)
![task3 2](https://github.com/user-attachments/assets/64596fac-8b6e-4b1b-9417-89aaf52a2230)


📚 What I Learned
Learned how to use the -S flag with GCC to generate human-readable assembly.

Understood the basic structure of function prologues and epilogues in RISC-V.

Gained insight into how printf-like functionality is implemented in bare-metal by writing directly to memory-mapped I/O.

Realized why bare-metal code uses infinite loops to terminate programs safely.
# ✅ Task 4: Hex Dump & Disassembly

## 📝 Task Description
The objective of this task was to analyze the compiled ELF file (`hello.elf`) by:
- Generating a disassembly listing (to see memory addresses, opcodes, and mnemonics)
- Creating a hex dump in Intel HEX format to view the raw byte-level contents of the binary
## ⚙️ What I Did
### 🔍 Disassembled ELF File
Used the following command to disassemble `hello.elf`:
```bash
riscv64-unknown-elf-objdump -d hello.elf > hello.dump
```
✅ This produced hello.dump, which contains:
Instruction addresses
Machine (hex) code
RISC-V assembly mnemonics

📄 Example from hello.dump:
```
80400000: 1141       addi sp, sp, -16
```
Explanation:
* 80400000:          //Memory address of the instruction
* 1141:              //Raw machine code in hexadecimal
* addi sp, sp, -16:  //The actual RISC-V instruction

💾 Created Intel HEX Dump
To generate a hex-format file:

```bash
riscv64-unknown-elf-objcopy -O ihex hello.elf hello.hex
```
✅ This produced hello.hex, a text representation of the program’s binary content in Intel HEX format — useful for programming devices.
📂 Viewed Dump File Using less
```bash
less hello.dump
```
This allowed scrolling through the disassembly output inside the terminal.

📌 Typical view:
```
80400000 <_start>:
80400000: 1141       addi sp,sp,-16
80400002: c422       sw   s0,12(sp)
~
~
(END)
```
Note:
~ denotes unused lines (not part of file)
(END) means you're at the end of the output
Press q to quit the viewer

📃 Optional: View Without Scroll
```bash
cat hello.dump
```
This directly prints the full disassembly in the terminal without needing to scroll.
![Screenshot from 2025-06-06 18-19-53](https://github.com/user-attachments/assets/8666a552-d1f4-4e79-950c-64e0e91bd709)

![Screenshot from 2025-06-06 18-15-40](https://github.com/user-attachments/assets/b85154d6-5337-4c10-aa3b-7ddff68690d5)

📚 What I Learned
Learned how to use objdump -d to inspect RISC-V ELF binaries and understand memory layout, opcodes, and instruction mapping.

Understood how to read Intel HEX files using objcopy -O ihex, which is commonly used in embedded systems.

