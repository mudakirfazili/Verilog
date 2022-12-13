# Verilog

16:1 MUX
```
module m161(out, S1, S2, S3, S4, D1, D2, D3, D4, D5, D6, D7, D8, D9, D10, D11, D12, D13, D14, D15, D16);
input S1, S2, S3, S4;
input D1, D2, D3, D4, D5, D6, D7, D8, D9, D10, D11, D12, D13, D14, D15, D16;
output reg out;
wire y1, y2;
m81 g1(y1, D1, D2, D3, D4, D5, D6, D7, D8, S1, S2, S3 );
m81 g2(y2, D9, D10, D11, D12, D13, D14, D15, D16, S1, S2, S3 );
m21 g3(y1, y2, S4, out);
endmodule 
```

3:8 Decoder
```
module d38(in, out);
input [2:0]in;
output [7;0]out;
wire w1, w2, w3;
NOT g1(in[0], w1);
NOT g2(in[1], w2);
NOT g3(in[2], w3);
NAND g4(out[0], w1, w2, w3);
NAND g5(out[1], w1, w2, C);
NAND g6(out[2], w1, B, w3);
NAND g7(out[3], w1, B, C);
NAND g8(out[4], A, w2, w3);
NAND g9(out[5], A, w2, C);
NAND g10(out[6], A, B, w3);
NAND g11(out[7], A, B, C);
endmodule
```


UP - DOWN Counter
```
module up_down_counter(out, up_down, clk, data, reset);
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
