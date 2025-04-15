🧠 MIPS32 Pipelined Processor (Verilog)
This project implements a 5-stage pipelined MIPS32 processor in Verilog, along with a testbench to simulate and verify its functionality. The processor supports basic arithmetic, memory operations, and branching, with a two-phase clock system (clk1 and clk2).

📁 Files

File Name	Description
mips32.v	Main processor module (pipe_MIPS32). Implements IF, ID, EX, MEM, WB stages.
mips32_test.v	Testbench to simulate the MIPS processor behavior.
mips32.vcd	Generated VCD (waveform) file from the testbench (if dumping is enabled).
🏗️ Pipeline Stages
IF (Instruction Fetch) – Fetches instruction from memory. Supports branch redirection.

ID (Instruction Decode) – Decodes opcode and reads registers.

EX (Execute) – Executes ALU operations or calculates memory/branch addresses.

MEM (Memory Access) – Loads or stores data to memory.

WB (Writeback) – Writes results back to registers.

🧾 Supported Instructions

Opcode	Mnemonic	Type	Description
000000	ADD	RR_ALU	Register + Register
000001	SUB	RR_ALU	Register - Register
000010	AND	RR_ALU	Bitwise AND
000011	OR	RR_ALU	Bitwise OR
000100	SLT	RR_ALU	Set if less than
000101	MUL	RR_ALU	Multiply
001010	ADDI	RM_ALU	Register + Immediate
001011	SUBI	RM_ALU	Register - Immediate
001100	SLTI	RM_ALU	Set if less than (imm)
001000	LW	LOAD	Load word from memory
001001	SW	STORE	Store word to memory
001101	BNEQZ	BRANCH	Branch if not equal to zero
001110	BEQZ	BRANCH	Branch if equal to zero
111111	HLT	HALT	Halt processor
🧪 Testbench Details (mips32_test.v)
✅ Initialization
Register bank is initialized with values 0 to 31.

Instruction memory is loaded with:

ADDI, LW, ADDI, SW, and HLT instructions.

Dummy instructions (OR R3, R3, R3) inserted for pipeline delays.

🧾 Sample Program:
assembly
Copy
Edit
ADDI  R1, R0, 120      // R1 = 120
LW    R2, 0(R1)        // R2 = Mem[120] = 80
ADDI  R2, R2, 45       // R2 = R2 + 45 = 125
SW    R2, 1(R1)        // Mem[121] = R2 = 125
HLT                   // Stop execution
🖨️ Output:
After simulation:

bash
Copy
Edit
MEM[120] = 80 
MEM[121] = 125
▶️ How to Run
bash
Copy
Edit
iverilog -o mysim mips32.v mips32_test.v
vvp mysim
To view waveform:

bash
Copy
Edit
gtkwave mips32.vcd
🛠️ Features
5-stage pipeline with data forwarding control via TAKEN_BRANCH.

Branch handling (BEQZ, BNEQZ) implemented.

Immediate sign-extension.

Memory-mapped instruction/data memory (1024 x 32-bit).

# MIPS32-PROCESSOR-
