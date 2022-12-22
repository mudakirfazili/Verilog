# Verilog

NOT Gate
```
module noot(y,a);
	input a;
	output y;
	assign y = ~a;
endmodule
```

NOT Gate (Behavioral)
```
module notb(y,ybar);
	input y;
	output reg ybar;
	always begin
		#5 ybar = ~y;
	end
endmodule
```

NOT Gate (LUT)
```
primitive not_1(y,a);
	input a;
	output y;
	table //a	:	y 
		0	:	1 ;
		1	:	0 ;
	endtable 
endprimitive 
```

NOT Gate (Testbench)
```
module noot_tst_bch();
	wire y;
	reg a;
	initial begin 
		#10 {a}= 'b 0 ;
		#10 {a}= 'b 1 ;
	end 
	noot a1(y,a);
endmodule 
```

OR Gate
```
module orr(y,a,b);
	input a,b;
	output y;
	assign y=a|b;
endmodule
```

OR Gate Testbench
```
module orr_tst_bch();
	wire y;
	reg a,b,c;
	initial begin 
		#5 {a,b,c} =  0;
		#5 {a,b,c} =  1;
		#5 {a,b,c} =  2;
		#5 {a,b,c} =  3;
		#5 {a,b,c} =  4;
		#5 {a,b,c} =  5;
		#5 {a,b,c} =  6;
		#5 {a,b,c} =  7;
	end
	mux2_1 tst(y,a,b,c);
endmodule
```

AND Gate
```
module andd(output y,input a,b);
	assign y=a^b;
endmodule
```

NOR Gate
```
module norb(y,a,b);
	input a,b;
	output reg y; 
	always begin
		#1 y = ~(a|b);
	end
endmodule
```

4-bit XOR
```
module par(y,i);
	input [3:0]i;
	output y;
	xor (y,i[0],i[1],i[2],i[3]);
	endmodule
```

Half Adder
```
module ha(sum,cout,a,b);
	input a,b;
	output sum,cout;
	assign sum=a^b;
	assign cout=a&b;
endmodule
```

Half Adder
```
module ha(sum,cout,a,b);
	input a,b;
	output sum,cout;
	xor (sum,a,b);
	and (cout,a,b);
endmodule
```

Half Adder (Using NAND)
```
module half_adder_nand(sum,cout,a,b);
	input a,b;
	output sum,cout;
	wire t1,t2,t3,t4;
	nand a1(t1,a,b);
	nand a2(t2,t1,a);
	nand a3(t3,t1,b);
	nand a4(sum,t2,t3);
	nand a5(t4,a,b);
	nand a6(cout,t4,t4);
endmodule 
```

Half Adder
```
module hacata(sum,cout,a,b);
	output wire sum,cout;
	input a,b;
	assign sum = {a+b};
	assign cout = {a*b};
endmodule
```

Full Adder
```
module fa2_1(sum,cout,a,b,cin);
	input a,b,cin;
	output sum,cout;
	wire [0:2]t;
	xor x1(t[0],a,b);
	xor x2(sum,t[0],cin);
	and a1(t[1],t[0],cin);
	and a2(t[2],a,b);
	or a3(cout,t[1],t[2]);
endmodule 
```

Full Adder (Behavioral)
```
module fa1b(sum,cout,a,b,cin);
	input a,b,cin;
	output reg cout,sum;
	reg t1,t2,t3;
	always begin @(a,b,cin)
		t1=a^b;
		sum=t1^cin;
		t2=cin&t1;
		t3=a&b;
		cout=t2|t3;
	end 
endmodule
```

3-bit Full Adder (using half adder)
```
module fa_ha_3(output [2:0]sum,output cout,input [2:0]a,b,input cin);
	wire [0:10]t;
	ha a1(sum[0],t[0],cin,t[8]);
	ha a2(t[8],t[1],a[0],b[0]);
	or o1(t[2],t[0],t[1]);
	ha a3(sum[1],t[3],t[2],t[9]);
	ha a4(t[9],t[4],a[1],b[1]);
	or o2(t[5],t[3],t[4]);
	ha a5(sum[2],t[6],t[5],t[10]);
	ha a6(t[10],t[7],a[2],b[2]);
	or o3(cout,t[6],t[7]);
endmodule
```

3-bit Full Adder (Testbench)
```
module fa3_1tst();
	reg clk,cin;
	reg [0:2]a,b;
	wire cout;
	wire [0:2]sum;
	integer count;
	fa_ha_3 aint(sum,cout,a,b,cin);
	initial 
		clk=1;
	initial
		count=0;
	always 
		#5 clk=~clk;
	always @(posedge clk)
	begin
		count=count+1;
	end
	always @(posedge clk)
	begin
		{cin,a,b}=count;
	end
endmodule
```

8-bit Full Adder (using 2-bit Full Adder)
```
module fa8_1(output [7:0]sum, output cout, input [7:0]a,b, input cin);
	wire [0:6]temp;
	fa2_1 f0(sum[0],temp[0],a[0],b[0],cin);
	fa2_1 f1(sum[1],temp[1],a[1],b[1],temp[0]);
	fa2_1 f2(sum[2],temp[2],a[2],b[2],temp[1]);
	fa2_1 f3(sum[3],temp[3],a[3],b[3],temp[2]);
	fa2_1 f4(sum[4],temp[4],a[4],b[4],temp[3]);
	fa2_1 f5(sum[5],temp[5],a[5],b[5],temp[4]);
	fa2_1 f6(sum[6],temp[6],a[6],b[6],temp[5]);
	fa2_1 f7(sum[7],cout,a[7],b[7],temp[6]);
endmodule 
```














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

Adder - Subtractor
```
module adder_subtractor(S, C, V, A, B, Op);
output [3:0] S;
output       C;
output       V;
input [3:0]  A;      
input [3:0]  B;
input        Op;

wire	     X0, X1, X2, X3, W0, W1, W2, W3;

xor(W0, B[0], Op);
xor(W1, B[1], Op);
xor(W2, B[2], Op);
xor(W3, B[3], Op);
xor(C, X3, Op);
xor(V, X3, Op);

Full_Adder f1( A[0], W0, Op, S[0], X0);
Full_Adder f2( A[1], W1, Op, S[1], X1);
Full_Adder f3( A[2], W2, Op, S[2], X2);
Full_Adder f4( A[3], W3, Op, S[3], X3);

endmodule
```
