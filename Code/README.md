# ALU-Control ROM Generator

This folder contains **`generate_ALUControl_rom.py`**, a small Python utility that outputs a Logisim-compatible `v2.0 raw` file implementing the MIPS ALU-control truth table used in this project.

---

## 📄 What the script does

* Enumerates every 8-bit address (`0x00‥0xFF`) interpreted as:
  * **Op1**  = address bit 7  
  * **Op0**  = address bit 6  
  * **F5–F0** = address bits 5–0 (R-type function field)
* Applies the priority rules ➜ returns a **4-bit control nibble**:
  1. `ALUOp = 11`  → `0000` (0)
  2. `Op0  = 1`    → `0110` (6)
  3. `ALUOp = 00`  → `0010` (2)
  4. `ALUOp = 10`  + decode `F3–0`
* Writes 256 hex digits to `ALUControl.rom` with the mandatory header:

```
 v2.0 raw
 <16 nibbles per line…>
```

Load this file into **`ALU_Control_ROM.circ` → Contents → Load…**.

---

## 🔧 Requirements

| Requirement | Notes |
|-------------|-------|
| Python ≥ 3.10 | Uses the `match … case` syntax. |
| No external libraries | Pure standard library. |

---

## 🚀 Quick Start

```bash
cd scripts
python3 generate_ALUControl_rom.py     # produces ../ALUControl.rom
```

*The script prints a success message and places the file one level up so Logisim can load it from the project root.*

---

## ⚙️ Customizing the Truth Table

1. Open **`generate_ALUControl_rom.py`**.
2. Edit `alu_control_code()` to add or change cases.
3. Run the script again – the ROM image will be regenerated automatically.

For large tables you may prefer to replace the `match` block with a dictionary lookup or import a CSV.

---

## 🧐 File Anatomy

```text
scripts/
├── generate_ALUControl_rom.py   # main script
└── README.md                    # this file
```

The generated **`ALUControl.rom`** lives in the project root by default.

---

## ❓ Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|------|
| **Logisim shows wrong outputs** | ROM not re-loaded after regenerating | Right-click ROM → *Reload* or re-select the file. |
| *SyntaxError: invalid syntax (match)* | Python < 3.10 | Upgrade Python or rewrite `match` as `if/elif`. |
| All zeros at runtime | Address lines wired incorrectly | Verify bit 7 → Op1, bit 6 → Op0, bits 5-0 → F5-F0. |

---

## 📝 License

MIT — same as the rest of the repository. Share, modify, and have fun!
