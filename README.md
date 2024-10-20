# SISO

Design code:


module SISO (
    input wire clk,        
    input wire rst,        
    input wire in,         
    output reg out         
);

reg [3:0] shift_reg;   
always @(posedge clk or posedge rst) begin
    if (rst) begin
        shift_reg <= 4'b0000;  
        out <= 0;
    end else begin
        shift_reg <= {shift_reg[2:0], in};  
        out <= shift_reg[3];               
    end
end
endmodule

Test bench:

module tb_SISO();
reg clk;
reg rst;
reg in;
wire out;
SISO uut (
    .clk(clk),
    .rst(rst),
    .in(in),
    .out(out)
);

always #5 clk = ~clk;  
initial begin
    clk = 0;
    rst = 0;
    in = 0;
    rst = 1;
    #10;
    rst = 0;
    #10;
    in = 1;  #10;
    in = 0;  #10;
    in = 1;  #10;
    in = 1;  #10;
    in = 0;  #10;
    in = 1;  #10;
    in = 0;  #10;
    $stop;
end
initial begin
    $monitor("At time %t: in = %b, out = %b", $time, in, out);
end
endmodule

