# Flipflop-using-Blocking-and-Non-blocking-Assignments
Exp 3 -Write and simulate D, SR, JK, T Flipflops using Blocking and Non blocking Assignments
   Aim: To design and simulate D, SR, JK, T Flipflops using Blocking and Non blocking Assignments in Verilog HDL and verify its functionality through a testbench using the Vivado 2023.1 simulation environment.
   
  Apparatus Required:
  Vivado 2023.1
Procedure:

Launch Vivado Open Vivado 2023.1 by double-clicking the Vivado icon or searching for it in the Start menu. 
Create a New Project Click on "Create Project" from the Vivado Quick Start window. In the New Project Wizard: Project Name: Enter a name for the project (e.g., Mux4_to_1). 
Project Location: Select the folder where the project will be saved. Click Next. 
Project Type: Select RTL Project, then click Next. Add Sources: Click on "Add Files" to add the Verilog files (e.g., mux4_to_1_gate.v, mux4_to_1_dataflow.v, etc.). 
Make sure to check the box "Copy sources into project" to avoid any external file dependencies. 
Click Next.
Add Constraints: Skip this step by clicking Next (since no constraints are needed for simulation). 
Default Part Selection: You can choose a part based on the FPGA board you are using (if any). 
If no board is used, you can choose any part, for example, xc7a35ticsg324-1L (Artix-7). 
Click Next, then Finish.
Add Verilog Source Files In the "Sources" window, right-click on "Design Sources" and select Add Sources if you didn't add all files earlier.
Add the Verilog files and the testbench. 
Check Syntax Expand the "Flow Navigator" on the left side of the Vivado interface. 
Simulate the Design In the Flow Navigator, under "Simulation", click on "Run Simulation" → "Run Behavioral Simulation". 
Vivado will open the Simulations Window, and the waveform window will show the signals defined in the testbench. 
View and Analyze Simulation Results Adjust Simulation Time To run a longer simulation or adjust timing, go to the Simulation Settings by clicking "Simulation" → "Simulation Settings". Under "Simulation", modify the Run Time (e.g., set to 1000ns).
Generate Simulation Report Once the simulation is complete, you can generate a simulation report by right-clicking on the simulation results window and selecting "Export Simulation Results".
Save the report for reference in your lab records. Save and Document Results Save your project by clicking File → Save Project.
Take screenshots of the waveform window and include them in your lab report to document your results. 
You can include the timing diagram from the simulation window showing the correct functionality of the Seven Segment across different select inputs and data inputs. 
Close the Simulation Once done, by going to Simulation → "Close Simulation


## RTL CODE:
D Flip Flop
```verilog
module d_ff (
    input D,
    input clk,
    input rst,
    output reg Q
);

always @(posedge clk or posedge rst) begin
    if (rst)
        Q <= 0;
    else
        Q <= D;
end

endmodule
```

SR Flip Flop
```verilog
module sr_ff (
    input S,
    input R,
    input clk,
    input rst,
    output reg Q
);

always @(posedge clk or posedge rst) begin
    if (rst)
        Q <= 0;
    else begin
        case ({S,R})
            2'b00: Q <= Q;
            2'b01: Q <= 0;
            2'b10: Q <= 1;
            2'b11: Q <= 1'bx;
        endcase
    end
end

endmodule
```

JK Flip Flop
```verilog
module jk_ff (
    input J,
    input K,
    input clk,
    input rst,
    output reg Q
);

always @(posedge clk or posedge rst) begin
    if (rst)
        Q <= 0;
    else begin
        case ({J,K})
            2'b00: Q <= Q;
            2'b01: Q <= 0;
            2'b10: Q <= 1;
            2'b11: Q <= ~Q;
        endcase
    end
end

endmodule
```

T Flip Flop
```verilog
module t_ff (
    input T,
    input clk,
    input rst,
    output reg Q
);

always @(posedge clk or posedge rst) begin
    if (rst)
        Q <= 0;
    else if (T)
        Q <= ~Q;
    else
        Q <= Q;
end

endmodule
```


## TestBench:

D Flip Flop
```verilog
`timescale 1ns/1ps
module tb_d_ff;

reg D, clk, rst;
wire Q;

d_ff uut (
    .D(D),
    .clk(clk),
    .rst(rst),
    .Q(Q)
);

always #5 clk = ~clk;

initial begin
    clk = 0; rst = 1; D = 0;
    #10 rst = 0;
    #10 D = 1;
    #10 D = 0;
    #10 D = 1;
    #20 $finish;
end

endmodule
```

SR Flip Flop
```verilog
`timescale 1ns/1ps
module tb_sr_ff;

reg S, R, clk, rst;
wire Q;

sr_ff uut (
    .S(S),
    .R(R),
    .clk(clk),
    .rst(rst),
    .Q(Q)
);

always #5 clk = ~clk;

initial begin
    clk = 0; rst = 1; S = 0; R = 0;
    #10 rst = 0;
    #10 S = 0; R = 0;
    #10 S = 1; R = 0;
    #10 S = 0; R = 1;
    #10 S = 1; R = 1;
    #10 S = 0; R = 0;
    #20 $finish;
end

endmodule
```

JK Flip Flop
```verilog
`timescale 1ns/1ps
module tb_jk_ff;

reg J, K, clk, rst;
wire Q;

jk_ff uut (
    .J(J),
    .K(K),
    .clk(clk),
    .rst(rst),
    .Q(Q)
);

always #5 clk = ~clk;

initial begin
    clk = 0; rst = 1; J = 0; K = 0;
    #10 rst = 0;
    #10 J = 0; K = 0;
    #10 J = 0; K = 1;
    #10 J = 1; K = 0;
    #10 J = 1; K = 1;
    #20 $finish;
end

endmodule
```

T Flip Flop
```verilog
`timescale 1ns/1ps
module tb_t_ff;

reg T, clk, rst;
wire Q;

t_ff uut (
    .T(T),
    .clk(clk),
    .rst(rst),
    .Q(Q)
);

always #5 clk = ~clk;

initial begin
    clk = 0; rst = 1; T = 0;
    #10 rst = 0;
    #10 T = 0;
    #10 T = 1;
    #10 T = 1;
    #10 T = 0;
    #10 T = 1;
    #20 $finish;
end

endmodule
```


## Output waveform:

D Flip Flop
![WhatsApp Image 2025-09-16 at 20 37 19_167b616a](https://github.com/user-attachments/assets/44cf1980-4b48-4c39-a15c-da90f5797ad7)


SR Flip Flop
![WhatsApp Image 2025-09-16 at 21 02 36_40748ab0](https://github.com/user-attachments/assets/95bba1e6-cfe5-417d-823c-b337d0fe7a77)


JK Flip Flop
![WhatsApp Image 2025-09-16 at 20 47 11_34f2e35b](https://github.com/user-attachments/assets/9165fb4f-8640-46fb-99ee-b719943795d2)


T Flip Flop
![WhatsApp Image 2025-09-16 at 20 55 57_a212fef6](https://github.com/user-attachments/assets/99c897e2-69e3-4cb6-8feb-ef2834240ec1)


## Conclusion:

All four flip-flops (D, JK, T, SR) were successfully designed and simulated in Verilog using Vivado 2023.1. The outputs matched theoretical truth tables:

  D Flip-Flop stored input D on clock edge.

  JK Flip-Flop performed set, reset, hold, and toggle operations.

  T Flip-Flop toggled output on each clock when T=1.

  SR Flip-Flop performed set/reset operations with invalid state for S=R=1.

Thus, the functionality of basic sequential circuits was verified using simulation.

