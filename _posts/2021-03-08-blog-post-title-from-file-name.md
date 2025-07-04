## 🚀 Hello World from Scratch: Bringing Up a RISC-V Core from RTL to C
Welcome! If you're wondering how to get started with hardware design, you've landed in the right place.

This guide takes you from zero to running C programs on an FPGA, using an open-source RISC-V core. Whether you're a student, hobbyist, or engineer looking to understand the full hardware-software bring-up process, this post will walk you through the essentials — from writing Verilog to printing "Hello, World" on your console.

# 🎯 Goals
- Boot an FPGA with a CPU and memory

- Add peripherals like UART, GPIO, LEDs, or your own custom digital logic

- Run C and assembly programs on the system

- Finally... print "Hello, World" to the console!

# 🧰 Requirements
- Any FPGA board

- A system with Vivado installed

- Basic familiarity with Verilog, C, and assembly (If you're new to these — no worries! This guide is beginner-friendly.)

Preferably a Linux system (Windows can work too, with some extra setup)

# 🛠️ Steps Overview
- Set up the PicoRV32 core and connect peripherals

- Install the RISC-V toolchain (riscv-gcc, etc.)

- Write, compile, and load C programs into memory

- Watch your console light up with:
```bash
Hello, World
```
