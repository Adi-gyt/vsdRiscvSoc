# ✅ Task 6: Stepping with GDB 🧪

## 🎯 Task Goal

The objective of this task was to step through the RISC-V program execution using `riscv64-unknown-elf-gdb`, set breakpoints, inspect registers, and understand low-level control flow in a bare-metal environment.

---
### 🧩 1. Start GDB

```bash
riscv64-unknown-elf-gdb hello.elf
```
This launches GDB with the compiled ELF file.

🧩 2. Connect to Built-in Simulator
```(gdb)
target sim
```
This tells GDB to use its internal RISC-V simulator (no hardware or QEMU required).

🧩 3. Set a Breakpoint
```(gdb)
 break _start
```
This sets a breakpoint at the _start label (the program entry point).

You can also use any symbol present in hello.s.

🧩 4. Run the Program
```(gdb)
 run
```
Execution starts and halts at the specified breakpoint.

🧩 5. Step Through Instructions
```(gdb)
 stepi
```
Or simply:
```(gdb)
si
```
This steps through one machine instruction at a time.

Repeat to walk through the program’s execution.

🧩 6. Inspect Registers
```(gdb)
 info registers
```
To view specific registers:
```
(gdb) print $a0
(gdb) print $sp
(gdb) print $pc
```
This gives insight into data flow and instruction effects.

📘 What I Learned
How to launch and use riscv64-unknown-elf-gdb in a bare-metal setup.

How to interact with GDB’s internal simulator.

How to set and hit breakpoints at specific symbols (like _start).

How to step through instructions (stepi) and observe exact behavior.

How to monitor CPU state using info registers and print $<reg>.

This task deepened my understanding of instruction-level debugging, register tracking, and how bare-metal programs run at the lowest level.
