## Traffic Light Controller using Verilog HDL and Testbench Verification
# Aim
To design and simulate a traffic light controller using Verilog HDL, and verify its functionality through a testbench in the Vivado 2023.1 simulation environment. The objective is to control the traffic lights for a junction with a specific time-based sequence for Red, Yellow, and Green lights.

# Apparatus Required
Vivado 2023.1 or equivalent Verilog simulation tool.
Computer system with a suitable operating system.
FPGA board (optional for hardware verification).
# Procedure
Launch Vivado 2023.1:

Open Vivado and create a new project.
Design the Traffic Light Controller Verilog Code:

Write the Verilog code for the traffic light controller, using an FSM (Finite State Machine) to transition between Green, Yellow, and Red lights based on timing intervals.
Create the Testbench:

Write a testbench to simulate the traffic light controller. The testbench will check the sequence of light transitions based on time.
Add the Verilog Files:

Add the traffic light controller Verilog code and the testbench file to the project.
Run Simulation:

Run the behavioral simulation in Vivado to verify the correct sequence of the traffic lights.
Observe the Waveforms:

Examine the waveform output to verify that the traffic light transitions through the Green, Yellow, and Red lights in the correct sequence.
Save and Document Results:

Capture screenshots of the waveform and save the simulation logs to include in your report.

# Verilog Code for Traffic Light Controller
```
module traffic_light_controller(
    input clk,       // Clock signal
    input reset,     // Reset signal
    output reg [2:0] lights // 3-bit output Red, Yellow, Green lights
);

        reg [1:0] state;
    
    // Timer counter for state transitions
    reg [3:0] timer;

    // State transition logic
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            state <= 2'b00; // Red state
            timer <= 4'd0;
        end
	 else
	 begin
            if (timer == 4'd9) begin // Change state after 10 clock cycles
                case (state)
                    2'b00: begin
                        state <= 2'b10; // Green state
                        timer <= 4'd0;
                    end
                    2'b10: begin
                        state <= 2'b01; // Yellow state
                        timer <= 4'd0;
                    end
                    2'b01: begin
                        state <= 2'b00; // Red state
                        timer <= 4'd0;
                    end
                endcase
            end
	 else
	 begin
                timer <= timer + 1;
            end
        end
    end

    // Output logic
    always @(*) begin
        case (state)
            2'b00:    lights = 3'b100; // Red
            2'b10:    lights = 3'b001; // Green
            2'b01:    lights = 3'b010; // Yellow
            default:  lights = 3'b100; // Default to Red
        endcase
    end

endmodule
```
# Output: ![traffic light controller](https://github.com/user-attachments/assets/81252d51-f854-462e-8e91-a81aecf3759a)

# Testbench for Traffic Light Controller
```
`timescale 1ns / 1ps

module tb_traffic_light_controller;
    reg clk;
    reg reset;
    wire [2:0] lights;

    // Instantiate the traffic light controller
    traffic_light_controller uut (
        .clk(clk),
        .reset(reset),
        .lights(lights)
    );

    // Clock generation
    always #5 clk = ~clk; // 10 time units clock period

    // Testbench process
    initial begin
        // Initialize signals
        clk = 0;
        reset = 1;

        // Apply reset for 10 time units
        #5 reset = 0;

        // Run simulation for 200 time units
        #200 $finish;
    end

    // Monitor output values
    initial begin
        $monitor("Time: %0t, Lights: %b", $time, lights);
    end
endmodule
```

# Conclusion
In this experiment, a traffic light controller was successfully designed and simulated using Verilog HDL. The design controlled the traffic lights to switch between Green, Yellow, and Red in a cyclic manner based on timing intervals. The testbench verified that the traffic lights followed the correct sequence and timing. The simulation results confirm the correct functionality of the traffic light controller, demonstrating the effectiveness of Verilog HDL in designing FSM-based controllers for real-world applications.
