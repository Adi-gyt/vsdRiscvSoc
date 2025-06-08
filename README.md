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

[Screenshot 2025-06-08 172121](https://github.com/user-attachments/assets/c62fdee7-3b88-4afb-b637-05a26ad72dbd)

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
Ensured correct PATH setup by restarting the terminal and checking tool accessibility.
Documented the installed version and confirmed it sup!(https://github.com/user-attachments/assets/8e211691-199c-4efb-9ccd-aca71628ba9f)
ports rv32imac architecture.
---

