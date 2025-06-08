# vsdRiscvSoc

## 🧠 VSD RISC-V SoC Journey

[![RISC-V](https://img.shields.io/badge/Architecture-RISC--V-blue.svg)](https://riscv.org/)
[![Toolchain](https://img.shields.io/badge/Toolchain-RISC--V%20GCC%20%7C%20SiFive-blueviolet.svg)](https://github.com/sifive/freedom-tools)
[![Platform](https://img.shields.io/badge/Platform-Ubuntu%2022.04.5-orange.svg)](https://ubuntu.com/download/desktop)
[![Progress](https://img.shields.io/badge/Progress-Started%20🚀-green.svg)]()

---

## 🚀 Project Overview

This repository documents my hands-on learning and implementation journey through the **VSD RISC-V SoC Labs** — an industry-aligned program focused on front-to-back SoC design. The course is structured into seven progressive phases, culminating in the **tape-out and physical fabrication** of a custom RISC-V board on the SCL180 process node.

> 🧩 *The final goal? A fully verified SoC design that runs on a real chip — the [VSDSquadron Board](https://www.vlsisystemdesign.com/vsdsquadron/).*

---

## 🔍 Current Focus

I'm currently working through the **initial toolchain bring-up and environment setup** — the first critical step toward developing, compiling, debugging, and eventually simulating bare-metal RISC-V programs.

> All progress is logged task-by-task with clean sub-directory breakdowns and detailed READMEs for traceability and scalability.

---

## 📅 Week-wise Breakdown

| ✅ Week | 🔍 Summary | 🔗 Link |
|--------|------------|--------|
| Week 1 | Installed RISC-V GCC toolchain on Ubuntu, verified cross-compilation binaries, created initial C program and ELF, disassembled it, debugged using GDB, explored objdump + objcopy, wrote inline assembly, and set up emulators like Spike and QEMU. | [Read Full Week 1 Log](./Phase%201/week%201/ReadMe.md) |

---

## 🔧 Learning Objectives

The weekly tasks (starting with Week 1) are aligned with the **RTL bring-up** stage of the VSD flow. Key goals:

- ✅ Setup the RISC-V cross-compilation toolchain
- ✅ Compile and analyze ELF binaries
- ✅ Disassemble and interpret RISC-V machine code
- ✅ Run code under `spike` and `qemu` emulators
- ✅ Use GDB to inspect register states
- ✅ Understand linker scripts, memory-mapped I/O, and startup code
- ✅ Implement low-level tasks like reading CSRs and simulating interrupts

---

## 📦 Folder Structure

```bash
vsdRiscvSoc/
├── README.md                 # Main summary (this file)
└── Phase 1/
    └── Week 1/
        └── ReadMe.md         # Full breakdown of 17 key tasks from RTL bring-up
```
---
> _Started with a printf, ending with a chip. Let's get start_
