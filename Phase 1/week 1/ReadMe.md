# Week 1: RISC-V Toolchain Setup & Bare-Metal Basics on Ubuntu 22.04 🚀🐧

Welcome to the RISC-V journey! This week’s doc will walk you through getting your dev setup ready, compiling and analyzing your first bare-metal program, and diving into some serious assembly vibes. We’re going all-in on the low-level magic. 🔥

---

## 🔧 Task 1: RISC-V Toolchain Setup on Ubuntu 22.04

### 🎯 Goal

Install the RISC-V toolchain, configure environment variables, and ensure tools like `gcc`, `objdump`, and `gdb` work properly for RV32 development.

### 📋 Requirements

* Ubuntu 22.04.5 (or compatible Linux distro)
* RISC-V toolchain: `riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz`

### 🚦 Setup Steps

#### 🧩 Step 1: Extract the Toolchain

```bash
cd ~/Downloads
tar -xzf riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz -C $HOME
```

Assume it extracts to `$HOME/riscv`.

#### 🛠️ Step 2: Add to PATH

```bash
echo 'export PATH=$HOME/riscv/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

#### 🔍 Step 3: Sanity Check

```bash
riscv32-unknown-elf-gcc --version
riscv32-unknown-elf-objdump --version
riscv32-unknown-elf-gdb --version
```

You should see version outputs confirming successful setup.

---

## ⚙️ Task 2: Compile “Hello, RISC-V”

### 🧠 Objective

Write and cross-compile a simple "Hello World" C program for RV32IMC using proper `gcc` flags.

### 👨‍💻 Steps

1. Create a basic `hello.c`:

```c
#include <stdio.h>
int main() {
    printf("Hello, RISC-V!\n");
    return 0;
}
```

2. Compile with RISC-V GCC:

```bash
riscv32-unknown-elf-gcc -march=rv32imc -mabi=ilp32 -o hello.elf hello.c
```

3. Confirm it's RV32 ELF:

```bash
file hello.elf
```

You should see: `ELF 32-bit LSB executable, UCB RISC-V, ...`

---

## 📜 Task 3: From C to Assembly

### 🔍 Objective

Generate human-readable assembly from your C code and explain key instructions.

### 🔧 Command

```bash
riscv32-unknown-elf-gcc -S -O0 hello.c
```

This creates `hello.s`. Look at `main:` — you'll see prologue like:

```asm
addi sp,sp,-16
sw ra,12(sp)
```

Explanation:

* `addi sp,sp,-16`: allocate stack
* `sw ra,12(sp)`: save return address

---

## 🧾 Task 4: Hex Dump & Disassembly

### 📌 Objective

View raw instructions and hex output from your compiled ELF.

### 💻 Commands

```bash
riscv32-unknown-elf-objdump -d hello.elf > hello.dump
riscv32-unknown-elf-objcopy -O ihex hello.elf hello.hex
```

Each disassembly line includes:

* Address
* Opcode (hex)
* Instruction mnemonic
* Operands

---

## 📚 Task 5: ABI & Register Cheat-Sheet

| Register | ABI Name | Role                  |
| -------- | -------- | --------------------- |
| x0       | zero     | Constant 0            |
| x1       | ra       | Return address        |
| x2       | sp       | Stack pointer         |
| x3       | gp       | Global pointer        |
| x4       | tp       | Thread pointer        |
| x5-x7    | t0-t2    | Temporaries (caller)  |
| x8       | s0/fp    | Saved / Frame ptr     |
| x9       | s1       | Saved (callee)        |
| x10-x17  | a0-a7    | Function args/returns |
| x18-x27  | s2-s11   | Saved (callee)        |
| x28-x31  | t3-t6    | Temporaries (caller)  |

Summary:

* `a0-a7` → args/returns
* `sX` → callee-saved
* `tX` → caller-saved

---

## 🧠 Task 6: Stepping with GDB

### 🧪 Objective

Debug your ELF with `gdb`, break at main, and inspect registers.

### 🐾 Steps

```bash
riscv32-unknown-elf-gdb hello.elf
target sim
break main
run
info reg a0
disassemble
```

---

## 🧿 Task 7: Running Under an Emulator

### 🧑‍🔬 Objective

Run your ELF without hardware using Spike or QEMU and view UART output.

### 🦔 Option A: Spike

```bash
spike --isa=rv32imc pk hello.elf
```

* Requires `pk` (proxy kernel)

### 🐧 Option B: QEMU

```bash
qemu-system-riscv32 -nographic -machine virt -kernel hello.elf
```

* Simulates UART output directly in terminal

---

## ⚡ Task 8: GCC Optimisation Effects

Compile twice:

```bash
riscv32-unknown-elf-gcc -S -O0 hello.c -o hello_O0.s
riscv32-unknown-elf-gcc -S -O2 hello.c -o hello_O2.s
```

Compare outputs:

* `-O2` removes dead code
* Inlines short functions
* Smarter register use

---

## 💥 Task 9: Inline Assembly — Read Cycle Counter

```c
static inline uint32_t rdcycle(void) {
    uint32_t c;
    asm volatile("csrr %0, cycle" : "=r"(c));
    return c;
}
```

Breakdown:

* `volatile`: prevents reordering
* `=r`(c): output constraint into register

---

## 🔌 Task 10: Memory-Mapped I/O Toggle

```c
volatile uint32_t *gpio = (uint32_t*)0x10012000;
*gpio = 0x1;
```

* `volatile` tells compiler not to optimize away writes
* Used for direct hardware register access

---

## 📍 Task 11: Minimal Linker Script

```ld
SECTIONS {
  .text 0x00000000 : { *(.text*) }
  .data 0x10000000 : { *(.data*) }
}
```

Explanation:

* `.text` → Flash
* `.data` → SRAM

---

## 🔁 Task 12: Start-up Code (crt0.S)

* Sets up stack pointer
* Clears `.bss`
* Calls `main`
* Hangs if `main` returns

Source: Use minimal `crt0.S` from newlib or other bare-metal examples

---

## 🧭 Task 13: Machine-Timer Interrupt (MTIP)

* Write to `mtimecmp`
* Enable `mie` for timer
* Set `mstatus.MIE`

ISR:

```c
void __attribute__((interrupt)) timer_handler(void) {
  // do something
}
```

---

## 🧬 Task 14: rv32imac vs rv32imc

* The `A` extension = Atomics
* Adds `lr.w`, `sc.w`, `amoadd.w`, etc.
* Crucial for threading, OS kernels

---

## 🔐 Task 15: Atomic Mutex with LR/SC

Pseudo-code:

```c
while (1) {
  if (__sync_lock_test_and_set(&lock, 1) == 0) {
    // critical section
    lock = 0;
    break;
  }
}
```

Under the hood: uses `lr/sc` to implement spinlocks

---

## 📤 Task 16: Newlib printf Without OS

* Implement `_write(int fd, char* buf, int len)`
* Send each byte to UART TX reg

Link with:

```bash
riscv32-unknown-elf-gcc -nostartfiles -T linker.ld -o hello.elf hello.c syscalls.c
```

---

## 📦 Task 17: Endianness & Struct Packing

Check byte order:

```c
union {
  uint32_t val;
  uint8_t bytes[4];
} u;

u.val = 0x01020304;
printf("%02x %02x %02x %02x\n", u.bytes[0], u.bytes[1], u.bytes[2], u.bytes[3]);
```

Expected: `04 03 02 01` → confirms little-endian

---

> Week 1 down. You’re now officially in control of your RISC-V dev setup, from toolchains to inline assembly. Let’s goo 🚀
