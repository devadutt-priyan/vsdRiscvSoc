# 🚀 RISCV Task 2: Prove Your Local RISCV Setup

This task showcases my setup by compiling, running, and dissecting several RISCV programs — proving that my RISCV toolchain and Spike simulator are up and running perfectly.

---

## 🎯 What’s Inside?

- **Four unique C programs** written to demonstrate different aspects of RISCV programming:
  - Factorial calculator
  - Maximum finder in an array
  - Bitwise operations showcase
  - Bubble sort implementation

- **Unique build identity:**  
  Each program prints the username, hostname, machine ID, build timestamps, and proof hashes — making every build truly unique.

- **Assembly & disassembly:**  
  Generated `.s` files and extracted disassembly snippets give a peek into the inner workings of the compiled code.

- **Manual instruction decoding:**  
  I've decoded at least five RISCV integer instructions per program, breaking down their binary representation and functionality.

---

## 🔧 Toolchain & Simulator

- **RISCV GCC Version:**
riscv64-unknown-elf-gcc (SiFive GCC 8.3.0-2019.08.0) 8.3.0
Copyright (C) 2018 Free Software Foundation, Inc.
This is free software; see the source for copying conditions. There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE

---

## Spike Simulator

Built from riscv-pk repository at commit: `fafaedd2825054222ce2874bf4a90164b5b071d4`

---
## 🛠 Build & Run

```bash
export U=$(id -un)
export H=$(hostname -s)
export M=$(head -c 16 /etc/machine-id)
export T=$(date -u +%Y-%m-%dT%H:%M:%SZ)
export E=$(date +%s)
```

---

## Compile & Run Commands
Compile each program using these commands:

```bash
riscv64-unknown-elf-gcc -O0 -g -march=rv64imac -mabi=lp64 \
-DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
-DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E factorial.c -o factorial

riscv64-unknown-elf-gcc -O0 -g -march=rv64imac -mabi=lp64 \
-DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
-DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E max_array.c -o max_array

riscv64-unknown-elf-gcc -O0 -g -march=rv64imac -mabi=lp64 \
-DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
-DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E bitops.c -o bitops

riscv64-unknown-elf-gcc -O0 -g -march=rv64imac -mabi=lp64 \
-DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
-DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E bubble_sort.c -o bubble_sort
```
Run each compiled program on Spike:

```bash
spike pk ./factorial
spike pk ./max_array
spike pk ./bitops
spike pk ./bubble_sort
```

---

## 🔍 Instruction Decoding
For this task, several RISC-V integer instructions from the compiled programs were manually decoded and documented. This demonstrates an understanding of the instruction format and helps verify the compiled output.

The decoded instructions and their details are included in the repository.

---

## Proof of Unique Build
Each program prints a header including:

+ Username
+ Hostname
+ Machine ID
+ Build UTC and Epoch time
+ GCC version
+ ProofID (unique per build)
+ RunID (unique per execution)

Screenshots of these outputs are included as:
+ <program>_output.png
+ <program>_main_asm.png
