# ✅ Task 8: Exploring GCC Optimization 🎯

## 📝 Task Goal

Understand the impact of compiler optimizations in GCC by comparing RISC-V assembly output of the same program compiled with and without optimizations.
## ⚙️ Steps Followed

### 🔧 1. Compiled Without Optimization (`-O0`)

```bash
riscv32-unknown-elf-gcc -S -O0 hello.c -o hello_O0.s
```
This version includes all instructions, comments, and intermediate steps.

Prioritizes readability and debugging over performance.

🚀 2. Compiled With Optimization (-O2)
```bash
riscv32-unknown-elf-gcc -S -O2 hello.c -o hello_O2.s
```
This version is optimized for speed and efficiency.

Removes unused code and simplifies instruction flow.

🔍 3. Compared the Files
```bash
diff hello_O0.s hello_O2.s
```
This command shows line-by-line differences between the two versions of the assembly code.

📊 Observations from diff Output
Aspect	-O0 Version	-O2 Version
Instruction Count	More, includes setup & padding	Significantly fewer
Dead Code	Present	Removed
Register Usage	Conservative, more saved/restored	Efficient reuse
Function Call	Standard call to printf()	Often optimized or partially inlined
Prologue/Epilogue	Present	Simplified or skipped

📘 What I Learned
The -O2 optimization level performs dead code elimination, constant folding, loop unrolling, and more.

Optimized assembly is harder to read but far more efficient.

The compiler intelligently avoids storing values in memory unless necessary and reuses registers.

Observed noticeable reduction in stack operations and instructions in the -O2 version.

This demonstrates how compilers balance performance vs. clarity depending on optimization levels.

