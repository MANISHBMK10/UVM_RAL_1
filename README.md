# APB Register Verification using UVM RAL

## 📌 Overview
This project demonstrates **UVM-based verification of a memory-mapped register block**
using an **APB-like bus protocol** and the **UVM Register Abstraction Layer (RAL)**.

The environment verifies correct **read/write behavior** of control and data registers
using:
- UVM sequences
- RAL model
- Adapter and predictor
- Scoreboard-based data checking

---

## 🧠 Key Concepts Demonstrated
- APB-style bus transactions (`psel`, `penable`, `pwrite`)
- UVM Agent (Driver, Monitor, Sequencer)
- UVM Register Model (`uvm_reg`, `uvm_reg_block`)
- RAL Adapter (`uvm_reg_adapter`)
- Register Predictor (`uvm_reg_predictor`)
- Frontdoor register access using RAL
- Scoreboard-based data validation

---

## 🧱 DUT Description
The DUT implements a simple register block:
| Address | Register |
|-------|----------|
| 0x00  | Control (4-bit) |
| 0x04  | Reg1 (32-bit) |
| 0x08  | Reg2 (32-bit) |
| 0x0C  | Reg3 (32-bit) |
| 0x10  | Reg4 (32-bit) |

Registers are accessed via an APB-like interface.

---

## 🧪 Verification Environment
- **Driver**: Generates APB read/write transactions
- **Monitor**: Observes bus activity and sends transactions to scoreboard and predictor
- **Scoreboard**: Compares expected vs actual read data
- **RAL Model**: Abstracts registers and enables frontdoor access
- **Adapter**: Converts RAL transactions to APB bus transactions
- **Predictor**: Keeps RAL mirror synchronized with DUT

---

## ▶️ Running the Simulation

This project supports simulation on both **Cadence Xcelium** and **Synopsys VCS** using UVM and RAL.

### 📌 Command
Run from the project root directory:
```bash
# ================================
# Cadence Xcelium (xrun)
# ================================
xrun -64bit -sv -uvm \
     -timescale 1ns/1ps \
     -f sim/files.f \
     +UVM_TESTNAME=test \
     +UVM_VERBOSITY=UVM_MEDIUM \
     -access +rwc \
     -gui


# ================================
# Synopsys VCS
# ================================
vcs -full64 -sverilog \
    -ntb_opts uvm \
    -timescale=1ns/1ps \
    -debug_access+all \
    -f sim/files.f \
    -l compile.log

./simv +UVM_TESTNAME=test +UVM_VERBOSITY=UVM_MEDIUM -gui
