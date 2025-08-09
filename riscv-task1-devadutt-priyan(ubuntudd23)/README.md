# TASK1: RISC-V Toolchain Setup & Uniqueness Test – VSD SoC Task

This task contains all the steps I followed to set up the RISC-V GCC toolchain, spike, proxy kernel, and iverilog. It includes the final uniqueness test script to validate setup.

## 📌 Base System Info

- OS: Ubuntu 24.04
- Terminal: Bash
- Shell variables used: `$pwd`, `$RISCV`, `$HOME`

---

## 🧰 Task 1 — Install Base Developer Tools

Install standard build tools and waveform viewers:

```bash
sudo apt-get install -y git vim autoconf automake autotools-dev curl \
libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex \
texinfo gperf libtool patchutils bc zlib1g-dev libexpat1-dev gtkwave
```
---

## 📁 Task 2 — Create Workspace & Capture Home Path

```bash
cd
pwd=$PWD
mkdir -p riscv_toolchain
cd riscv_toolchain
```

---

## 📦 Task 3 — Get a Prebuilt RISC‑V GCC Toolchain

```bash
wget "https://static.dev.sifive.com/dev-tools/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14.tar.gz"
tar -xvzf riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14.tar.gz
```

---

## 🛠️ Task 4 — Add Toolchain to PATH
Current shell:
```bash
export PATH=$pwd/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin:$PATH
```
Persistent:
```bash
echo 'export PATH=$HOME/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

---

## 🌳 Task 5 — Install Device Tree Compiler (DTC)

```bash
sudo apt-get install -y device-tree-compiler
```

---

## 💻 Task 6 — Build and Install Spike (ISA Simulator)

```bash
cd $pwd/riscv_toolchain
git clone https://github.com/riscv/riscv-isa-sim.git
cd riscv-isa-sim
mkdir -p build && cd build
../configure --prefix=$pwd/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14
make -j$(nproc)
sudo make install
```

---

## 🔗 Task 7 — Build and Install Proxy Kernel (PK)

```bash
cd $pwd/riscv_toolchain
git clone https://github.com/riscv/riscv-pk.git
cd riscv-pk
mkdir -p build && cd build
../configure --prefix=$pwd/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14 --host=riscv64-unknown-elf
make -j$(nproc)
sudo make install
```

---

## 🧩 Task 8 — Add Nested Cross Bin to PATH (for pk)
Current Shell:
```bash
export PATH=$pwd/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/riscv64-unknown-elf/bin:$PATH
```
Persistent:
```bash
echo 'export PATH=$HOME/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/riscv64-unknown-elf/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

---

## 🔧 Task 9 — Install Icarus Verilog

```bash
cd $pwd/riscv_toolchain
git clone https://github.com/steveicarus/iverilog.git
cd iverilog
git checkout --track -b v10-branch origin/v10-branch
git pull
chmod +x autoconf.sh
./autoconf.sh
./configure
make -j$(nproc)
sudo make install
```

---

## 🧪 Task 10 — Sanity Checks

```bash
which riscv64-unknown-elf-gcc
riscv64-unknown-elf-gcc -v

which spike
spike --version

which pk
```

---

## 🔬 Uniqueness Test
A simple C program was compiled using riscv64-unknown-elf-gcc and run using spike pk to validate the setup.
Check the unique_test folder in this repo for source code, compile command and output.

---

## Troubleshooting Notes
+ If -mcmodel=medany fails → you're accidentally using host gcc instead of riscv64 toolchain.

+ If configure: C compiler cannot create executables → recheck your $PATH, dependencies, and permissions.

+ Always verify extracted folder names; sometimes typos in $RISCV can silently break builds.

---

## ✅ Status
- [x] Toolchain installed

- [x] Paths configured

- [x] Spike and PK built and tested

- [x] Optional Verilog installed

- [x] Unique test passed
