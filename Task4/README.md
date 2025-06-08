# ✅ Task 3: Hex Dump & Disassembly

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

📚 What I Learned
Learned how to use objdump -d to inspect RISC-V ELF binaries and understand memory layout, opcodes, and instruction mapping.

Understood how to read Intel HEX files using objcopy -O ihex, which is commonly used in embedded systems.

Became familiar with terminal file viewers like less and how to navigate large outputs effectively.
