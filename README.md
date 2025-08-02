# MIPS 32-bit CPU in Logisim

Welcome to my digital logic and CPU design project! This repository contains the Logisim circuit files I use to build a 32-bit MIPS CPU from scratch.

🛠️ Created for educational purposes as part of the **Black Body Engineering** series on YouTube.

---

## 🧠 What’s Inside

This project is modular and grows with each episode. Current components:

```
circuits/
├── ALU.circ                 # 32-bit ALU (add, subtract, AND, OR, set-on-less-than)
├── Register_File.circ       # 32-register file with dual-read, single-write ports
├── Instruction_Memory.circ  # Read-only memory storing instructions
├── Program_Counter.circ     # 32-bit program counter for instruction sequencing
├── Data_Memory.circ         # 32-bit word-addressable read/write memory
├── ALU_Control_ROM.circ     # 8-bit-address ROM that outputs the 4-bit ALU control code
```

> **Source code & scripts**
>
> - `scripts/generate_ALUControl_rom.py` – Python script that regenerates the `v2.0 raw` image for `ALU_Control_ROM.circ`.
>
>   ```bash
>   python3 scripts/generate_ALUControl_rom.py   # outputs ALUControl.rom
>   ```
>   Load the resulting file into Logisim via **ROM ▸ Contents ▸ Load…**.

---

### ⚙️ ALU Operation Codes

The ALU takes a 2-bit control input `OPR` to select the arithmetic or logic operation. It also uses `AInvert` and `BNegate` to perform subtraction and bitwise inversion.

| OPR | Operation        | Description                         |
|-----|------------------|-------------------------------------|
| 00  | AND              | Bitwise AND of A and B              |
| 01  | OR               | Bitwise OR of A and B               |
| 10  | ADD or SUB       | Performs A + B or A − B             |
| 11  | Set-on-LessThan  | Outputs 1 if A < B, else 0          |

> 💡 **Notes**  
> • To perform subtraction, set `BNegate = 1` for two’s complement.  
> • `AInvert = 1` inverts input A (used in some logic configurations).  
> • Overflow, carry, and zero flags are included for status tracking.

---

### 🏗️ ALU Control ROM (Control Logic)

MIPS uses a small combinational block to turn the primary `ALUOp` field plus the R-type function code (`funct`) into a 4-bit control word sent to the ALU.  In hardware we implement that block as an 8-bit-address, 4-bit-data **ROM**.

```
addr[7] = Op1      (ALUOp1)
addr[6] = Op0      (ALUOp0)
addr[5] = F5
addr[4] = F4
addr[3] = F3
addr[2] = F2
addr[1] = F1
addr[0] = F0
```

| ALUOp | Funct (F3-0) | ALU-Control | Hex |
|-------|--------------|-------------|-----|
| 00    | xxxxxx       | 0010 (ADD)  | 2   |
| x1    | xxxxxx       | 0110 (SUB)  | 6   |
| 10    | 0000         | 0010 (ADD)  | 2   |
| 10    | 0010         | 0110 (SUB)  | 6   |
| 10    | 0100         | 0000 (AND)  | 0   |
| 10    | 0101         | 0001 (OR)   | 1   |
| 10    | 1010         | 0111 (SLT)  | 7   |

The **Python generator** enumerates every 8-bit address, decodes the bits back into `(Op1, Op0, funct)`, applies the table rules, and writes a `v2.0 raw` file with 256 hex nibbles.  You can adjust the table inside the script and rerun it any time you change the instruction set.

---

### 🧾 Register File

The `Register_File.circ` implements a 32-register file where each register is 32 bits wide.

**Key Features:**
- **Dual-read, single-write** architecture  
- Controlled by `RegWrite` and `CLK` signals for synchronous write  
- Addressed using 5-bit `Read_Reg_1`, `Read_Reg_2`, and `WriteReg` inputs  
- Input to register through the 32-bit `WriteData` port  
- Outputs `Output_1` and `Output_2` for parallel register access

---

### 📜 Instruction Memory

The `Instruction_Memory.circ` stores the CPU’s instructions in a read-only memory module.

**Key Features:**
- 32-bit wide instruction output  
- Addressed by the 32-bit Program Counter input  
- Supports sequential and jump instruction fetching  

⚠️ **Simulation Limitation**

Due to Logisim's internal constraints, the instruction memory and program counter simulation is limited to **128 MB** of addressable space instead of the full 4 GB supported by a true 32-bit MIPS CPU. This is because Logisim supports a maximum of 24 input pins for the address bus in these components.

---

### 🔄 Program Counter

The `Program_Counter.circ` is a 32-bit register that keeps track of the current instruction address.

**Key Features:**
- Synchronous increment or jump control  
- Controlled by clock (`CLK`) and reset signals  
- Provides the address to fetch the next instruction from Instruction Memory  

---

### 🧮 Data Memory

The `Data_Memory.circ` provides word-addressable memory for reading and writing data during program execution.

**Key Features:**
- 32-bit wide address, read, and write data ports  
- Synchronous write using the `MemWrite` control signal  
- Asynchronous read from the `ReadData` output  
- Used for load/store instructions like `LW` and `SW` in MIPS

⚠️ **Simulation Limitation**

Due to Logisim's internal constraints, Data Memory is limited to a maximum of 24 address-bus pins.

#### 🧪 Simulating `LW` and `SW` Instructions

**Inputs:**
- `Address` (32-bit): Memory address to read/write  
- `WriteData` (32-bit): Value to write (`SW`)  
- `MemWrite` (1-bit): Enable writing on positive clock edge  
- `CLK`: Clock input (only needed for `SW`)  

**Output:**
- `ReadData` (32-bit): Value read from `Address` (`LW`)

##### `SW` (Store Word)
1. Set `Address` (e.g. `0x00000008`)  
2. Set `WriteData` (e.g. `0x000000FF`)  
3. `MemWrite = 1`, then apply rising edge to `CLK`  
4. Set `MemWrite = 0` afterwards

##### `LW` (Load Word)
1. Set `Address`  
2. Ensure `MemWrite = 0`  
3. `ReadData` shows the 32-bit word stored at that address

> 🧠 *Tip*: Reads are combinational; no clock needed for `LW`.

---

## 🚀 How to Use

1. Download **Logisim Evolution** <https://github.com/logisim-evolution/logisim-evolution>  
2. Clone or download this repository  
3. Open any `.circ` file in Logisim Evolution  
4. Simulate the circuits by applying inputs and observing outputs  
5. *(Optional)* Regenerate the ALU-control ROM:
   ```bash
   python3 scripts/generate_ALUControl_rom.py
   ```

---

## 🎓 Learn with Me

This project is part of my educational series on YouTube:  
📺 **Black Body Engineering** – <https://youtube.com/@BlackBodyEngineering>

Each episode builds on the previous module:

```
Partial Full Adder → CLA → ALU → Register File → Instruction Memory → Program Counter → Data Memory → Control Logic → CPU
```

---

## 📝 License

MIT License – Free to use, share, and modify.  
If you use this project in a class or tutorial, please credit the channel!

---

## 🙌 Contributions / Feedback

Corrections, improvements, and extensions are welcome.  
Open an issue or pull request – or just say hi on YouTube comments!
