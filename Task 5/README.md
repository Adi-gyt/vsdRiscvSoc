# ✅ Task 5: ABI & Register Cheat Sheet 

## 📝 Task Description

The objective of this task was to learn and document the 32 RISC-V general-purpose integer registers, understand their ABI (Application Binary Interface) names, and analyze their roles based on RISC-V calling conventions.

---

## 📋 Step 1: Register Table

| Register | ABI Name | Description           | Role (Calling Convention)  |
|----------|----------|-----------------------|-----------------------------|
| x0       | zero     | Hard-wired 0          | Constant 0                  |
| x1       | ra       | Return address        | Caller-saved                |
| x2       | sp       | Stack pointer         | Callee-saved                |
| x3       | gp       | Global pointer        | —                           |
| x4       | tp       | Thread pointer        | —                           |
| x5       | t0       | Temporary             | Caller-saved                |
| x6       | t1       | Temporary             | Caller-saved                |
| x7       | t2       | Temporary             | Caller-saved                |
| x8       | s0/fp    | Saved / Frame Pointer | Callee-saved                |
| x9       | s1       | Saved                 | Callee-saved                |
| x10      | a0       | Arg 1 / return value  | Caller-saved                |
| x11      | a1       | Arg 2 / return value  | Caller-saved                |
| x12      | a2       | Arg 3                 | Caller-saved                |
| x13      | a3       | Arg 4                 | Caller-saved                |
| x14      | a4       | Arg 5                 | Caller-saved                |
| x15      | a5       | Arg 6                 | Caller-saved                |
| x16      | a6       | Arg 7                 | Caller-saved                |
| x17      | a7       | Arg 8                 | Caller-saved                |
| x18      | s2       | Saved                 | Callee-saved                |
| x19      | s3       | Saved                 | Callee-saved                |
| x20      | s4       | Saved                 | Callee-saved                |
| x21      | s5       | Saved                 | Callee-saved                |
| x22      | s6       | Saved                 | Callee-saved                |
| x23      | s7       | Saved                 | Callee-saved                |
| x24      | s8       | Saved                 | Callee-saved                |
| x25      | s9       | Saved                 | Callee-saved                |
| x26      | s10      | Saved                 | Callee-saved                |
| x27      | s11      | Saved                 | Callee-saved                |
| x28      | t3       | Temporary             | Caller-saved                |
| x29      | t4       | Temporary             | Caller-saved                |
| x30      | t5       | Temporary             | Caller-saved                |
| x31      | t6       | Temporary             | Caller-saved                |

---

## 📌 Step 2: Calling Convention Summary

### 🔹 a0–a7 (x10–x17)
- **Purpose**: Function arguments and return values
- **Convention**: Caller-saved

### 🔹 s0–s11 (x8–x9, x18–x27)
- **Purpose**: Saved registers (used for variables that persist across function calls)
- **Convention**: Callee-saved

### 🔹 t0–t6 (x5–x7, x28–x31)
- **Purpose**: Temporaries
- **Convention**: Caller-saved (can be used freely without saving)

### 🔹 Special Registers
- **ra (x1)**: Return address
- **sp (x2)**: Stack pointer
- **fp (alias of s0)**: Frame pointer


## 📚 What I Learned

- RISC-V uses **32 general-purpose registers (x0–x31)**.
- Each has an **ABI name** and a specific role in the **calling convention**.
- Learned the difference between:
  - **Caller-saved registers**: Must be saved by the caller if needed.
  - **Callee-saved registers**: Preserved across function calls by the called function.
- This understanding is essential for:
  - Writing and debugging assembly code
  - Understanding function call behavior
  - Managing memory and stack frames properly
