# 1:8 Demultiplexer using Verilog

## 📌 Project Overview

This project implements a **1:8 Demultiplexer (DEMUX)** using Verilog HDL.

A demultiplexer is a combinational circuit that takes one input signal and routes it to one of multiple output lines based on the select inputs.

A 1:8 DEMUX contains:

* One data input
* Three select inputs
* Eight outputs

---

## 🎯 Objective

The objective of this project is to design and verify a 1:8 demultiplexer using Verilog HDL.

The design is verified using a dedicated Verilog testbench.

---

## 🛠️ Technologies Used

* Verilog HDL
* VS Code
* Icarus Verilog / ModelSim / QuestaSim
* GTKWave (optional)
* GitHub

---

## 📂 Project Structure

```text
1-to-8-Demultiplexer/
│
├── README.md
├── src/
│   └── demux_1to8.v
├── testbench/
│   └── tb_demux_1to8.v
└── simulation/
    └── simulation_results.txt
```

---

## 🔢 Inputs and Outputs

| Signal     | Description   |
| ---------- | ------------- |
| D          | Data input    |
| S2, S1, S0 | Select inputs |
| Y0–Y7      | Output lines  |

---

## 📋 Selection Table

| S2 | S1 | S0 | Selected Output |
| -- | -- | -- | --------------- |
| 0  | 0  | 0  | Y0              |
| 0  | 0  | 1  | Y1              |
| 0  | 1  | 0  | Y2              |
| 0  | 1  | 1  | Y3              |
| 1  | 0  | 0  | Y4              |
| 1  | 0  | 1  | Y5              |
| 1  | 1  | 0  | Y6              |
| 1  | 1  | 1  | Y7              |

When `D = 1`, the selected output becomes `1` and all other outputs remain `0`.

When `D = 0`, all outputs remain `0`.

---

## ⚙️ Operation

The select lines determine which output receives the input.

```text
S2 S1 S0

000 → Y0 = D
001 → Y1 = D
010 → Y2 = D
011 → Y3 = D
100 → Y4 = D
101 → Y5 = D
110 → Y6 = D
111 → Y7 = D
```

---

## 💻 Verilog Implementation

The design uses a `case` statement inside an `always @(*)` block.

First, all outputs are cleared:

```verilog
y = 8'b00000000;
```

Then the selected output is connected to the input:

```verilog
case (sel)

    3'b000: y[0] = d;
    3'b001: y[1] = d;
    3'b010: y[2] = d;
    3'b011: y[3] = d;
    3'b100: y[4] = d;
    3'b101: y[5] = d;
    3'b110: y[6] = d;
    3'b111: y[7] = d;

endcase
```

---

## 🧪 Testbench

The testbench verifies:

* All eight select combinations
* Data input `0`
* Data input `1`

A total of **16 test cases** are performed.

---

## ▶️ Simulation Using Icarus Verilog

Open the terminal inside the project directory.

### Compile

```bash
iverilog -o demux8_sim src/demux_1to8.v testbench/tb_demux_1to8.v
```

### Run

```bash
vvp demux8_sim
```

### Expected Output

```text
======================================================
                  1:8 DEMULTIPLEXER
======================================================
 D | S2 S1 S0 | Y7 Y6 Y5 Y4 Y3 Y2 Y1 Y0
------------------------------------------------------
 0 |  000   |    00000000
 0 |  001   |    00000000
 0 |  010   |    00000000
 0 |  011   |    00000000
 0 |  100   |    00000000
 0 |  101   |    00000000
 0 |  110   |    00000000
 0 |  111   |    00000000
 1 |  000   |    00000001
 1 |  001   |    00000010
 1 |  010   |    00000100
 1 |  011   |    00001000
 1 |  100   |    00010000
 1 |  101   |    00100000
 1 |  110   |    01000000
 1 |  111   |    10000000
======================================================
```

---

## 📚 Concepts Demonstrated

* Demultiplexer design
* Combinational logic
* Select lines
* `case` statement
* `always @(*)`
* Verilog vectors
* Module instantiation
* Testbench development
* Functional verification
* Digital simulation

---

## 🚀 Future Improvements

This project can be extended to:

* 1:16 Demultiplexer
* Parameterized DEMUX
* Gate-level DEMUX
* Hierarchical DEMUX
* FPGA implementation

---

## 👩‍💻 Author

**Harshu**

B.Tech - Electronics and Communication Engineering

---

## 📄 License

This project is created for educational and academic purposes.
```verilog
`timescale 1ns/1ps

module demux_1to8 (
    input  wire       d,
    input  wire [2:0] sel,
    output reg  [7:0] y
);

    always @(*) begin

        // Default: all outputs are 0
        y = 8'b00000000;

        case (sel)
            3'b000: y[0] = d;
            3'b001: y[1] = d;
            3'b010: y[2] = d;
            3'b011: y[3] = d;
            3'b100: y[4] = d;
            3'b101: y[5] = d;
            3'b110: y[6] = d;
            3'b111: y[7] = d;
        endcase

    end

endmodule
```
```verilog
`timescale 1ns/1ps

module tb_demux_1to8;

    reg       d;
    reg [2:0] sel;

    wire [7:0] y;

    // Instantiate the Design Under Test
    demux_1to8 DUT (
        .d(d),
        .sel(sel),
        .y(y)
    );

    initial begin

        $display("======================================================");
        $display("                  1:8 DEMULTIPLEXER");
        $display("======================================================");
        $display(" D | S2 S1 S0 | Y7 Y6 Y5 Y4 Y3 Y2 Y1 Y0");
        $display("------------------------------------------------------");

        // ------------------------------------------------
        // TEST CASES WITH D = 0
        // ------------------------------------------------

        d = 1'b0;

        sel = 3'b000;
        #10;
        $display(" %b |  %b   |    %b", d, sel, y);

        sel = 3'b001;
        #10;
        $display(" %b |  %b   |    %b", d, sel, y);

        sel = 3'b010;
        #10;
        $display(" %b |  %b   |    %b", d, sel, y);

        sel = 3'b011;
        #10;
        $display(" %b |  %b   |    %b", d, sel, y);

        sel = 3'b100;
        #10;
        $display(" %b |  %b   |    %b", d, sel, y);

        sel = 3'b101;
        #10;
        $display(" %b |  %b   |    %b", d, sel, y);

        sel = 3'b110;
        #10;
        $display(" %b |  %b   |    %b", d, sel, y);

        sel = 3'b111;
        #10;
        $display(" %b |  %b   |    %b", d, sel, y);


        // ------------------------------------------------
        // TEST CASES WITH D = 1
        // ------------------------------------------------

        d = 1'b1;

        sel = 3'b000;
        #10;
        $display(" %b |  %b   |    %b", d, sel, y);

        sel = 3'b001;
        #10;
        $display(" %b |  %b   |    %b", d, sel, y);

        sel = 3'b010;
        #10;
        $display(" %b |  %b   |    %b", d, sel, y);

        sel = 3'b011;
        #10;
        $display(" %b |  %b   |    %b", d, sel, y);

        sel = 3'b100;
        #10;
        $display(" %b |  %b   |    %b", d, sel, y);

        sel = 3'b101;
        #10;
        $display(" %b |  %b   |    %b", d, sel, y);

        sel = 3'b110;
        #10;
        $display(" %b |  %b   |    %b", d, sel, y);

        sel = 3'b111;
        #10;
        $display(" %b |  %b   |    %b", d, sel, y);

        $display("======================================================");

        $finish;

    end

endmodule
```
# 1:8 DEMULTIPLEXER SIMULATION RESULTS

D = 0

Select = 000 → Output = 00000000
Select = 001 → Output = 00000000
Select = 010 → Output = 00000000
Select = 011 → Output = 00000000
Select = 100 → Output = 00000000
Select = 101 → Output = 00000000
Select = 110 → Output = 00000000
Select = 111 → Output = 00000000

D = 1

Select = 000 → Output = 00000001
Select = 001 → Output = 00000010
Select = 010 → Output = 00000100
Select = 011 → Output = 00001000
Select = 100 → Output = 00010000
Select = 101 → Output = 00100000
Select = 110 → Output = 01000000
Select = 111 → Output = 10000000

======================================================
Simulation completed successfully.
==================================
