# Verilog

## NOT Gate
```
module noot(y,a);
	input a;
	output y;
	assign y = ~a;
endmodule
```

## NOT Gate (Behavioral)
```
module notb(y,ybar);
	input y;
	output reg ybar;
	always begin
		#5 ybar = ~y;
	end
endmodule
```

## NOT Gate (LUT)
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

## NOT Gate (Testbench)
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

## OR Gate
```
module orr(y,a,b);
	input a,b;
	output y;
	assign y=a|b;
endmodule
```

## OR Gate Testbench
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

## AND Gate
```
module andd(output y,input a,b);
	assign y=a^b;
endmodule
```

## NOR Gate
```
module norb(y,a,b);
	input a,b;
	output reg y; 
	always begin
		#1 y = ~(a|b);
	end
endmodule
```

## 4-bit XOR
```
module par(y,i);
	input [3:0]i;
	output y;
	xor (y,i[0],i[1],i[2],i[3]);
	endmodule
```

## Half Adder
```
module ha(sum,cout,a,b);
	input a,b;
	output sum,cout;
	assign sum=a^b;
	assign cout=a&b;
endmodule
```

## Half Adder
```
module ha(sum,cout,a,b);
	input a,b;
	output sum,cout;
	xor (sum,a,b);
	and (cout,a,b);
endmodule
```

## Half Adder (Using NAND)
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

## Half Adder
```
module hacata(sum,cout,a,b);
	output wire sum,cout;
	input a,b;
	assign sum = {a+b};
	assign cout = {a*b};
endmodule
```

## Full Adder
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

## Full Adder (Behavioral)
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

## 3-bit Full Adder (using half adder)
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

## 3-bit Full Adder (Testbench)
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

## 8-bit Full Adder (using 2-bit Full Adder)
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

## 2:1 MUX
```
module mux2_1(y,s0,i0,i1);
	input s0,i0,i1;
	output y;
	wire s0bar,t1,t2;
	not (s0bar,s0);
	and a1(t1,s0bar,i0);
	and a2(t2,s0,i1);
	or (y,t1,t2);
endmodule
```
## 4:1 MUX (using case statement)
```
module mux4_1_case(y,s,i);
	input [3:0]i;
	input [1:0]s;
	output reg y;
	always @(s,i)
	case (s)
		2'b00 : y = i[0];
		2'b01 : y = i[1];
		2'b10 : y = i[2];
		2'b11 : y = i[3];
		
	endcase
endmodule 
```

## 8:1 MUX (using 2:1 MUX)
```
module mux8_1(output y,input [0:2]s,input [0:7]i);
	wire [0:5]t;
	mux2_1 a1(t[0],s[0],i[0],i[1]);
	mux2_1 a2(t[1],s[0],i[2],i[3]);
	mux2_1 a3(t[2],s[0],i[4],i[5]);
	mux2_1 a4(t[3],s[0],i[6],i[7]);
	mux2_1 a5(t[4],s[1],t[0],t[1]);
	mux2_1 a6(t[5],s[1],t[2],t[3]);
	mux2_1 a7(y,s[2],t[4],t[5]);
endmodule 
```

## 8:1 MUX (Testbench)
```
module mux8_1tst();
	reg clk;
	reg [0:7]I;
	reg [0:2]s;
	integer count;
	mux8_1 ab(y,s,I);
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
		{s,I}=count;
	end
endmodule
```

## Gray to Binary converter
```
module g2b(output [3:0]b,input [3:0]g);
	assign b[3] = g[3];
	xor a1(b[2],g[2],g[3]);
	xor a2(b[1],g[1],b[2]);
	xor a3(b[0],g[0],b[1]);
endmodule
```

## Gray to Binary converter(Testbench)
```
module tstbch();
	wire [3:0]b;
	reg [3:0]g;
	reg clk;
	integer count ;
	gray2binary a(b,g);
	initial
		clk = 1;
	initial 
		count = 0;
	always 
		#10 clk = ~clk;
	always @(posedge clk)
	begin
		count = count + 1;
	end
	always @(posedge clk)
	begin
		{g} = count;
	end
endmodule
```

## BCD to 7 segement convereter
```
module seven(y,i);
	output reg [0:6]y;
	input [3:0]i;
	always @(i)
	case (i)
		4'b0000 : y = 7'b1111110;
		4'b0001 : y = 7'b0110000;
		4'b0010 : y = 7'b1101101;
		4'b0011 : y = 7'b1111001;
		4'b0100 : y = 7'b0110011;
		4'b0101 : y = 7'b1011011;
		4'b0110 : y = 7'b1011111;
		4'b0111 : y = 7'b1110000;
		4'b1000 : y = 7'b1111111;
		4'b1001 : y = 7'b1111101;
		default : y = 7'b0000000;
	endcase
endmodule 
```

## Priority Encoder
```
module pe4_1(y,i);
	input [0:3]i;
	output reg [0:1]y;
	always @(i)
	casez (i)
	 4'b1??? : y = 2'b11;
	 4'b01?? : y = 2'b10;
	 4'b001? : y = 2'b01;
	 4'b0001 : y = 2'b00;
	endcase
endmodule
```

## 8:3 Priority Encoder
```
module prio_enco(i,y);
	input [7:0]i;
	output reg [2:0]y;
	always @(i)
	if ( i[7] == 1'b1)
		y = 1;
	else if ( i[6] == 1'b1)
		y = 2;
	else if ( i[5] == 1'b1)
		y = 3;
	else if ( i[4] == 1'b1)
		y = 4;
	else if ( i[3] == 1'b1)
		y = 5;
	else if ( i[2] == 1'b1)
		y = 6;
	else if ( i[1] == 1'b1)
		y = 7;
	else if ( i[0] == 1'b1)
		y = 0;
endmodule
```

## Parity generator
```
module par_gen(y,i);
	input [7:0]i;
	output reg y;
	integer k;
	always @(i)
	begin
		y = i[0] ^ i[1]; 
	for (k=2 ; k<7; k= k+1)
		y = y ^ i[k] ;
	end
endmodule
```
## 
## 
## 
## 
## 
## 
## 
## 


## 16:1 MUX
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

## 3:8 Decoder
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


## UP - DOWN Counter
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

## Adder - Subtractor
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





# Author
[**Mohammad Mudakir Fazili**](https://www.linkedin.com/in/mudakirfazili14/), *M.Tech Micro-electronics*, NIT Srinagar                                                                                           
*mudakirfazili@gmail.com*


