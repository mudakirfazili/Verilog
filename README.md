# Verilog


UP - DOWN Counter
```
module up_down_counter_Aryan(out, up_down, clk, data, reset);
output [3;0] out;
input [3:0] data;
input up_down, clk, reset;
reg [3;0] out;
always @(negedge clk)
if(reset) begin
out <= 4'b0;
end else if(up_down) begin
out <= out + 1;
end else begin
out <= out - 1;
end
endmodule 
```
