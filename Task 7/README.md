## ✅ Task 7: Running Under an Emulator

### 🎯 Objective
Run a compiled RISC-V ELF program (`hello.elf`) using QEMU (or Spike) to simulate bare-metal execution and observe UART output.

🛠️ What I Did
🧾 Step 1: Wrote hello.c
```c
void main() {
    volatile char* uart = (char*)0x10000000;
    const char* msg = "Hello, RISC-V!\n";
    while (*msg) {
        *uart = *msg++;
    }
}
```
✅ This uses direct memory-mapped UART I/O instead of printf, avoiding standard library dependencies.

🧾 Step 2: Compiled for RISC-V 
```bash
riscv32-unknown-elf-gcc -march=rv32imc -mabi=ilp32 -nostdlib -ffreestanding \
  -Wl,-Ttext=0x80000000 -e main -o hello.elf hello.c
```
Flags used:

-nostdlib: No C library or startup files.

-ffreestanding: No assumptions about runtime environment.

-Ttext=0x80000000: Place program at specific memory address in RAM.

🧾 Step 3: Ran on QEMU
```bash
qemu-system-riscv32 -nographic -machine virt -bios none -kernel hello.elf

```
✅ Output observed in terminal:
```
Hello, RISC-V!
```

🔍 Why This Matters
📦 Embedded Programming: Bare-metal programming skills are essential for microcontroller and SoC development.

⚙️ Toolchain Mastery: Understanding how to compile, link, and run ELF files prepares you for real-world development on custom hardware.

🧠 Firmware Awareness: You now know when and why to include firmware like OpenSBI depending on your system needs.
