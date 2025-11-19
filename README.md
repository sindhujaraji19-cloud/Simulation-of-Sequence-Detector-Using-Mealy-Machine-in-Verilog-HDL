# **Simulation of Sequence Detector Using Mealy Machine in Verilog HDL**

---

## **Aim**
To design and simulate a **Sequence Detector using Mealy Machine** in Verilog HDL that detects a specific bit pattern from a given binary input sequence.

---

## **Apparatus Required**
- Computer with **Vivado Design Suite** installed  
---

## **Theory**

A **Sequence Detector** identifies a specific sequence of bits from a stream of binary input data.  
**Mealy Machine** – Output depends on both the current state and the input.

In the **Mealy Machine**, the output can change immediately in response to an input, making it faster than the Moore type.  
The Mealy model uses **state transitions** and **output logic** based on both the current state and the input signal.

For example, a **sequence detector** that detects `"1011"`:
- It produces an output `1` whenever the sequence `1011` appears in the input stream.
- Overlapping sequences are also detected.

---
## State Diagram 
<img width="363" height="550" alt="image" src="https://github.com/user-attachments/assets/7861102e-c301-422a-bfa8-d8d887a8652b" />

## Verilog Code
```
`timescale 1ns/1ps

module mealy_seq_detector_11011 (
    input clk,
    input reset,
    input x,
    output reg z
);
    parameter S0 = 3'b000,
              S1 = 3'b001,
              S2 = 3'b010,
              S3 = 3'b011,
              S4 = 3'b100,
              S5 = 3'b101;

    reg [2:0] state, next_state;

    always @(posedge clk or posedge reset) begin
        if (reset)
            state <= S0;
        else
            state <= next_state;
    end

    always @(*) begin
        case (state)
            S0: begin
                if (x) begin
                    next_state = S1;
                    z = 0;
                end else begin
                    next_state = S0;
                    z = 0;
                end
            end
            S1: begin
                if (x) begin
                    next_state = S2;
                    z = 0;
                end else begin
                    next_state = S0;
                    z = 0;
                end
            end
            S2: begin
                if (x) begin
                    next_state = S2;
                    z = 0;
                end else begin
                    next_state = S3;
                    z = 0;
                end
            end
            S3: begin
                if (x) begin
                    next_state = S4;
                    z = 0;
                end else begin
                    next_state = S0;
                    z = 0;
                end
            end
            S4: begin
                if (x) begin
                    next_state = S5;
                    z = 1;
                end else begin
                    next_state = S3;
                    z = 0;
                end
            end
            S5: begin
                if (x) begin
                    next_state = S2;
                    z = 0;
                end else begin
                    next_state = S3;
                    z = 0;
                end
            end
            default: begin
                next_state = S0;
                z = 0;
            end
        endcase
    end
endmodule

```
## Testbench
```
`timescale 1ns/1ps

module tb_mealy_seq_detector_11011;
    reg clk, reset, x;
    wire z;

    mealy_seq_detector_11011 uut (
        .clk(clk),
        .reset(reset),
        .x(x),
        .z(z)
    );

    initial begin
        clk = 0;
        forever #5 clk = ~clk;
    end

    initial begin
        reset = 1; x = 0;
        #10 reset = 0;

        // Input: 1 1 0 1 1 0 1 1 0 1
        x = 1; #10;
        x = 1; #10;
        x = 0; #10;
        x = 1; #10;
        x = 1; #10;
        x = 0; #10;
        x = 1; #10;
        x = 1; #10;
        x = 0; #10;
        x = 1; #10;

        #20 $finish;
    end

    initial begin
        $monitor("Time=%0t | X=%b | Z=%b | State=%b", $time, x, z, uut.state);
    end
endmodule

```
## Simulation Output 
---

<img width="1918" height="1198" alt="image" src="https://github.com/user-attachments/assets/3c1cb67a-036c-4ece-8e6e-d8be2b0cd8ea" />

---





## Result

The Mealy Machine Sequence Detector for the bit pattern "11011" was successfully designed and simulated using Verilog HDL.
Simulation results confirm that the circuit correctly detects the pattern and supports overlapping sequences.

