Task 17: Endianness Verification
Objective:
Verify how multi-byte values are stored in RISC-V memory to confirm the endianness.

Code Used:
A union of a 32-bit integer and 4 bytes was used to inspect the byte ordering of the number 0x12345678.

```c
union {
    uint32_t word;
    uint8_t bytes[4];
} u;

void _start() {
    u.word = 0x12345678;
    while (1);  // Halt here to inspect memory with GDB
}
```
Compilation Command:

```bash
riscv64-unknown-elf-gcc -Wall -g -nostartfiles -nostdlib -T link.ld \
  -msmall-data-limit=0 -march=rv32imac -mabi=ilp32 \
  endian.c -o task17.elf
```
Running and Debugging:
Run in QEMU with OpenSBI firmware and wait for GDB connection:

```bash
qemu-system-riscv32 -nographic -machine virt \
  -bios /media/sf_firmware/fw_jump.bin \
  -kernel task17.elf \
  -s -S
```
Connect GDB and inspect the union:

```gdb
(gdb) target remote :1234
(gdb) break _start
(gdb) continue
(gdb) print u.word
$1 = 305419896   # Decimal form of 0x12345678
(gdb) x/4bx &u
0x80200010: 0x78 0x56 0x34 0x12
```
Result:
The bytes are stored in memory as 0x78 0x56 0x34 0x12, confirming that RISC-V uses little-endian format (least significant byte at the lowest memory address).

What I Learned:

How multi-byte values are stored in memory on RISC-V hardware.

RISC-V uses little-endian byte ordering.

Importance of inspecting data layout when working with low-level embedded code.
