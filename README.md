# 🧠 MIPS32 Pipelined Processor (Verilog Implementation)

This project implements a 5-stage pipelined MIPS32 processor using Verilog. The processor supports a subset of MIPS instructions and demonstrates instruction flow through IF, ID, EX, MEM, and WB stages.

---

## 📁 Files

- `mips32.v`: The main pipelined processor module (`pipe_MIPS32`).
- `mips32_test.v`: The testbench that simulates the processor.
- `mips32.vcd`: Waveform output file (generated during simulation).

---

## 🚀 Processor: `pipe_MIPS32`

### ✅ Supported Instructions

| Type      | Mnemonic | Opcode     |
|-----------|----------|------------|
| ALU (RR)  | ADD      | `000000`   |
|           | SUB      | `000001`   |
|           | AND      | `000010`   |
|           | OR       | `000011`   |
|           | SLT      | `000100`   |
|           | MUL      | `000101`   |
| ALU (RM)  | ADDI     | `001010`   |
|           | SUBI     | `001011`   |
|           | SLTI     | `001100`   |
| Memory    | LW       | `001000`   |
|           | SW       | `001001`   |
| Branch    | BEQZ     | `001110`   |
|           | BNEQZ    | `001101`   |
| Special   | HLT      | `111111`   |

### 🧱 Pipeline Stages

1. **IF (Instruction Fetch)**
2. **ID (Instruction Decode)**
3. **EX (Execute)**
4. **MEM (Memory Access)**
5. **WB (Write Back)**

### 🔍 Control Signals
- `HALTED`: Signals the end of program execution.
- `TAKEN_BRANCH`: Used to handle control hazards by detecting branches.

---

## 🧪 Testbench: `mips32_test.v`

### 🕹️ Clock Generation (Two-Phase Clock)
```verilog
repeat(20) begin
    #5 clk1 = 1'b1; #5 clk1 = 1'b0;
    #5 clk2 = 1'b1; #5 clk2 = 1'b0;
end
```
- `clk1`: Triggers IF, EX, WB
- `clk2`: Triggers ID, MEM

### 🔧 Initialization
```verilog
for (k = 0; k < 32; k++)
    mips.regbank[k] = k;
```
- Register file initialized with index values
- Program Counter and control flags set

### 📜 Instruction Sequence
```assembly
0:  ADDI R1, R0, 120
1:  NOP
2:  LW R2, 0(R1)
3:  NOP
4:  ADDI R2, R2, 45
5:  NOP
6:  SW R2, 1(R1)
7:  HLT
```
- Dummy `OR R3,R3,R3` instructions simulate NOPs
- Demonstrates memory load, arithmetic, and memory store

### 🧾 Data Memory
```verilog
mips.Mem[120] = 80;  // Memory input
```

### ✅ Output Verification
```verilog
$display("MEM[120] = %4D \nMEM[121] = %4d", mips.Mem[120], mips.Mem[121]);
```
Expected:
```
MEM[120] = 80
MEM[121] = 125
```

### 📉 Waveform Dump
```verilog
$dumpfile("mips32.vcd");
$dumpvars(0, mips_test);
```
Use GTKWave to visualize pipeline execution.

---

## ▶️ Running Simulation

```bash
iverilog -o mysim mips32.v mips32_test.v
vvp mysim
```

> Ensure both `.v` files are in the same directory before running.

---

## 👀 Output Sample
```
MEM[120] =   80
MEM[121] =  125
```
This confirms correct execution of LW, ADDI, SW, and HALT instructions.

---

## 📈 Visualization
Open `mips32.vcd` with GTKWave:
```bash
gtkwave mips32.vcd
```
Observe signal propagation across pipeline stages.

---

## 📌 Conclusion
This MIPS32 implementation offers a basic yet functional 5-stage pipeline model, suitable for academic simulation of pipelining, hazards, control flow, and memory interaction in hardware design.

Feel free to extend the instruction set, add hazard detection, or implement forwarding!

