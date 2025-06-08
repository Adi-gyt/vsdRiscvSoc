# ✅ Task 12: Start-up Code & crt0 – Simulating Low-Level Boot
## 🎯 Objective

Understand the responsibilities of `crt0.S` (C RunTime zero) in embedded RISC-V programs and how to simulate its behavior with a manual `_start()` in C.

## 🧠 What crt0.S Typically Does

In most bare-metal programs, `crt0.S` is the **first assembly code that runs** before `main()` or `_start()` in C.

Its responsibilities:

- 🪜 Set up the **stack pointer (`sp`)**
- 🧹 Zero out the **`.bss` section** (uninitialized data)
- 🧾 Copy `.data` section from **Flash to RAM** (if needed)
- 🧰 Run global constructors (for C++)
- 🚀 Call `main()` or user-defined `_start()`
- 🔁 Loop forever or call `exit()`

## 💡 What I Did in This Task

I simulated the behavior of crt0.S by writing a manual _start() in C, which ran without error under QEMU. Since the program ran as expected and didn't require .bss or .data relocation, this successfully mimics a basic crt0 setup.
```c
void _start() {
    // Stack initialization skipped (QEMU handles it)
    setup_interrupts();  // Example of setup code

    while (1) {
        if (triggered) {
            count++;
            triggered = 0;
        }
    }
}
```
✅ Stack setup was skipped since QEMU auto-sets sp.

✅ No need to clear .bss or relocate .data in this minimal example.

✅ This serves as a working substitute for crt0.

📘 What I Learned
The role of crt0.S as the bare-metal boot entry point.

Its responsibility in preparing memory and calling into C code.

That a minimal _start() in C can substitute crt0.S under QEMU, for educational/testing purposes.

Understanding this flow is critical for writing bootloaders, OS kernels, or initializing microcontrollers.
