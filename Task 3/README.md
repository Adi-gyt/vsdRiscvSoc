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

🔍 Assembly Analysis
🔧 Function Prologue
The beginning of the main function sets up the stack and frame pointer:

```assembly
addi sp, sp, -16     # Allocate stack space
sw   s0, 12(sp)      # Save s0 (callee-saved register)
addi s0, sp, 16      # Establish frame pointer
```
🖨️ Print "Hello, RISC-V"
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

📚 What I Learned
Learned how to use the -S flag with GCC to generate human-readable assembly.

Understood the basic structure of function prologues and epilogues in RISC-V.

Gained insight into how printf-like functionality is implemented in bare-metal by writing directly to memory-mapped I/O.

Realized why bare-metal code uses infinite loops to terminate programs safely.


