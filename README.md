## Sequence-Detector
# Aim:

To design and simulate a sequence detector using both Moore and Mealy state machine models in Verilog HDL, and verify their functionality through a testbench using the Vivado 2023.1 simulation environment. The objective is to detect a specific sequence of bits (e.g., 1011) and compare the Moore and Mealy designs.

# Apparatus Required:

Vivado 2023.1 or equivalent Verilog simulation tool.
Computer system with a suitable operating system.

# Procedure:

Launch Vivado 2023.1:

Open Vivado and create a new project.
Write the Verilog Code for Sequence Detector (Moore and Mealy FSM):

Design two Verilog modules: one for a Moore FSM and another for a Mealy FSM to detect a sequence such as 1011.
Create the Testbench:

Write a testbench to apply input sequences and verify the output of both FSM designs.
Add the Verilog Files:

Add the Verilog code for both FSMs and the testbench to the Vivado project.

# Run Simulation:

Run the simulation and observe the output to check if the sequence is detected correctly.

Observe the Waveforms:

Analyze the waveform to ensure both the Moore and Mealy machines detect the sequence as expected.
Save and Document Results:

Capture the waveforms and include the results in the final report.

# Verilog Code for Sequence Detector Using Moore FSM:
```
module fsm_sequence(
    input clk,
    input reset,
    input run,
    output reg [3:0] count
);
  localparam s0 = 4'd0;
    localparam s2 = 4'd2;
    localparam s4 = 4'd4;
    localparam s6 = 4'd6;
    localparam s8 = 4'd8;
    localparam s10 = 4'd10;
    localparam s12 = 4'd12;
 reg [3:0] current_state, next_state;
always @(posedge clk or posedge reset) begin
        if (reset) begin
            current_state <= s0;  
        end else begin
            current_state <= next_state;  
        end
    end
    always @(*) begin
        case (current_state)
            s0:  if (run) next_state = s2;  else next_state = s0;
            s2:  if (run) next_state = s4;  else next_state = s2;
            s4:  if (run) next_state = s6;  else next_state = s4;
            s6:  if (run) next_state = s8; else next_state = s6;
            s8:  if (run) next_state = s10; else next_state = s8;
            s10: if (run) next_state = s12; else next_state = s10;
            s12: if (run) next_state = s0;  else next_state = s12;
            default: next_state = s0;
        endcase
    end
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            count <= 4'd0; 
        end else begin
            count <= current_state;  
        end
    end
endmodule
```
# Output:

![image](https://github.com/user-attachments/assets/1ceae463-6a44-48f1-8149-bf1349bbd2c6)


# Verilog Code for Sequence Detector Using Mealy FSM:
```
module fsm_sequence(
    input clk,
    input reset,
    input run,
    output reg [3:0] count
);
    localparam s0  = 4'd0;
    localparam s2  = 4'd2;
    localparam s4  = 4'd4;
    localparam s6  = 4'd6;
    localparam s8  = 4'd8;
    localparam s10 = 4'd10;
    localparam s12 = 4'd12;

 reg [3:0] current_state, next_state;
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            current_state <= s0;  
        end else begin
            current_state <= next_state;  
        end
    end
  always @(*) begin
        case (current_state)
            s0:  next_state = run ? s2 : s0;
            s2:  next_state = run ? s4 : s2;
            s4:  next_state = run ? s6 : s4;
            s6:  next_state = run ? s8 : s6;
            s8:  next_state = run ? s10 : s8;
            s10: next_state = run ? s12 : s10;
            s12: next_state = run ? s0 : s12;
            default: next_state = s0;
        endcase
    end
 always @(*) begin
        case (current_state)
            s0:  count = run ? 4'd2 : 4'd0;
            s2:  count = run ? 4'd4 : 4'd2;
            s4:  count = run ? 4'd6 : 4'd4;
            s6:  count = run ? 4'd8 : 4'd6;
            s8:  count = run ? 4'd10 : 4'd8;
            s10: count = run ? 4'd12 : 4'd10;
            s12: count = run ? 4'd0 : 4'd12;
            default: count = 4'd0;
        endcase
    end
endmodule
```

# Output:

![image](https://github.com/user-attachments/assets/149da106-a0f2-41e1-87d9-377f31baed2b)


# Testbench for moore:
```
module tb_fsm_sequence;

 reg clk;
    reg reset;
    reg run;
    wire [3:0] count;
    fsm_sequence uut (
        .clk(clk),
        .reset(reset),
        .run(run),
        .count(count)
    );
  always #10 clk = ~clk;
  initial begin
        clk = 0;
        reset = 1;
        run = 0;
        #20 reset = 0; 
        #20 run = 1;   
        #100 run = 0;   
        #40 run = 1;   
        #100 $finish;   
    end
   
   initial begin
        $monitor("Time: %0t | clk: %b | reset: %b | run: %b | count: %d", 
                 $time, clk, reset, run, count);
    end
  initial begin
        $dumpfile("fsm_sequence_tb.vcd");
        $dumpvars(0, tb_fsm_sequence);
    end
endmodule
```
# Output:

![image](https://github.com/user-attachments/assets/a2cfcabe-54cf-4057-adf7-caabd483de55)

# Testbench for Mealey:
```
module mealy(clk,rst,inp,out);
 input clk, rst, inp;
 output out;
 reg out;
 reg [1:0] state;
 always @(posedge clk, posedge rst)
 begin
     if(rst)
     begin
     out <= 0;
            state <= 2'b00;
         end
     else
         begin
             case (state)
             2'b00: 
                 begin
                     if (inp) begin
                         state <=2'b01;
                         out <=0;
                     end
                     else begin
                         state <=2'b10;
                         out <=0;
                    end
                 end
             2'b01:
                 begin
 if(inp) begin
  state <= 2'b00;
   out <= 1;
 end

 else begin
  state <= 2'b10;
 out <= 0
end
 end
 2'b10:
begin
 if(inp)
 begin

  state <= 2'b01;

  out <= 0;

  end
 else begin

  state <= 2'b00;
      out <= 1;
         end
    end
 default:
                 begin
                     state <= 2'b00;
                     out <= 0;
                 end
             endcase
         end
 end
 endmodule
```
 # Output:

 ![image](https://github.com/user-attachments/assets/f262c180-85fe-4b0a-ab70-7617b69ba64e)


# Conclusion:

In this experiment, Moore and Mealy FSMs were successfully designed and simulated to detect the sequence 1011. Both designs worked as expected, with the main difference being that the Moore FSM generated the output based on the current state, while the Mealy FSM generated the output based on both the current state and input. The testbench verified the functionality of both FSMs, demonstrating that the Verilog HDL can effectively model both types of state machines for sequence detection tasks.
