# MIPS 32-bit CPU in Logisim

Welcome to my digital logic and CPU design project! This repository contains the Logisim circuit files I use to build a 32-bit MIPS CPU from scratch.

🛠️ Created for educational purposes as part of the **Black Body Engineering** series on YouTube.

---

## 🧠 What’s Inside

This project is modular and grows with each episode. Current components:

### `circuits/`
- **alu.circ** – 1-bit and 4-bit ALU (add, subtract, AND, OR, set-on-less-than)

---

## ⚙️ ALU Operation Codes

The ALU takes a 2-bit control input `OPR` to select the arithmetic or logic operation. It also uses `AInvert` and `BNegate` to perform subtraction and bitwise inversion.

| OPR | Operation        | Description                    |
|-----|------------------|--------------------------------|
| 00  | AND              | Bitwise AND of A and B         |
| 01  | OR               | Bitwise OR of A and B          |
| 10  | ADD or SUB       | Performs A + B or A − B        |
| 11  | Set-on-LessThan  | Outputs 1 if A < B, else 0     |
|-----|------------------|--------------------------------|

> 💡 **Notes:**
> - To perform **subtraction**, set `BNegate = 1` for two’s complement.
> - `AInvert = 1` inverts input A (used in some logic configurations).
> - Overflow, carry, and zero flags are included for status tracking.

---

## 🚀 How to Use

1. Download [Logisim Evolution](https://github.com/logisim-evolution/logisim-evolution/releases)
2. Clone or download this repository
3. Open any `.circ` file in Logisim Evolution
4. Simulate the circuits by applying inputs and observing outputs

---

## 🎓 Learn with Me

This project is part of my educational series on YouTube:
📺 **Black Body Engineering**  
🔗 [https://youtube.com/@BlackBodyEngineering](https://youtube.com/@BlackBodyEngineering)

Each video walks through the design and logic behind each component:
- Full Adder → CLA → ALU → Control Logic → CPU
- Clear, step-by-step logic explanations
- Focused on electrical & computer engineering students

---

## 📝 License

MIT License – Free to use, share, and modify.  
If you use this in your own project or class, consider crediting the channel!

---

## 🙌 Contributions / Feedback

Feedback, corrections, or improvements are welcome.  
Feel free to open an issue or pull request—or just say hi on Reddit or YouTube.
