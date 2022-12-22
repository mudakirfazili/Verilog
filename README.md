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

## Priority Encoder
```
module pe_if(y,i);
	input [3:0]i;
	output reg [1:0]y;
	always @(i)
		if (i[0] == 1)
			y = 2'b00;
		else if (i[1] == 1)
			y = 2'b01;
		else if (i[2] == 1)
			y = 2'b10;
		else if (i[3] == 1)
			y = 2'b11;
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
## D flip flop
```
module ffll(q,qbar,rst,clk,d);
	input d,clk,rst;
	output reg q,qbar;
	always @(posedge (clk), posedge(rst))
		begin  if (rst == 1 )
			q = 0;
		else if (d == 0)
			q = 0;
		else if (d ==1)
			q = 1;
		 qbar = ~q;
		end
		
endmodule
```

## JK flip flop
```
module jkfftest(q,qbar,j,k);
	input j,k;
	output reg q,qbar;
	initial begin 
		q = 0;
		qbar=~q;
	end
	always begin
		#10 q=(j&(~q))|(~k&q);
		#10 qbar =~q;
	end
endmodule 
```

## JK flip flop (else-if)
```
module jkflip(j,k,clk,rst,q,qbar);
	input j,k,rst,clk;
	output q,qbar;
	reg temp;
	initial
		temp = 1'b0;
	always @(posedge clk ,posedge rst)
	begin	if (rst == 1'b0)
			temp = 1'b0;
		else if (j==0 & k==0)
			temp = temp;
		else if (j==0 & k==1)
			temp = 1'b0;
		else if (j==1 & k==0)
			temp = 1'b1;
		else if (j==1 & k==1)
			temp = ~temp;
	end
	assign q = temp;
	assign qbar = ~temp;
endmodule 
```

## Master Slave JK
```
module masterslave_jk(q,p,j,k,clk);
	input j,k,clk;
	inout p,q;
	wire t1,t2,t3;
	jk_ff a(t1,t2,j,k,clk);
	not c(t3,clk);
	jk_ff b(q,p,t1,t2,t3);
endmodule
```

## UP counter
```
module up_counter(clk,q,rst);
	input clk,rst;
	output [3:0]q;
	reg [3:0] count;
	initial
		count = 0; 
	always @(negedge clk , posedge rst)
		begin
		if (rst == 1'b1)
			count = 1'b0;
		else 
			count = count + 1;
		end
		assign q = count;
		
endmodule
```

## Shift register
```
module shift_reg(output [0:3]out,input [0:3]in);
	assign #5 out = in>>2;
	assign #5 out = in<<2;
endmodule
```

## Up counter (with load and pause)
```
module load_pause(load,pause,data,clk,q,rst);
	input clk,rst,load ,pause;
	input [3:0]data;
	output[3:0]q;
	reg [3:0] count;
	initial
		count = 0; 
	always @(posedge clk , posedge rst)
		begin
		if (rst == 1'b1)
			count = 1'b0;
		else if (load == 1'b1)
			count = data; 
		else if (pause == 1)
			count = count;
		else 
			count = count + 1;
		end
		assign q = count;
		
endmodule
```
## 8 bit multiplier
```
module em(res,inta,intb);
	parameter size = 8;
	input [size : 1]inta,intb;
	output [2*size : 1]res;
	reg [2*size : 1]temp_res,shift_a,shift_b;
	always @(*)
	begin 
		shift_a=inta;
		shift_b=intb;
		temp_res=0;
	repeat(size)
		begin
		if (shift_b[1])
			temp_res = temp_res + shift_a;
			shift_a = shift_a << 1;
			shift_b = shift_b >> 1;
		end
	end
	assign res = temp_res;
endmodule
```

## Frequency Divider
```
module fdc(clk,q2,q4,q8,rst);
	input clk ,rst ;
	output q2,q4,q8;
	reg temp2,temp4,temp8;
	initial begin
		temp2 = 0;
		temp4 = 0;
		temp8 = 0;
	end
	assign q2 = temp2;
	assign q4 = temp4;
	assign q8 = temp8;
	always @(posedge clk,posedge rst)
	begin 
		if (rst == 1'b1)
			 temp2 = 1'b0;
		else 
		 temp2 = ~temp2;
	end 
	
	always @(posedge q2)
		temp4 = ~temp4;
	always @(posedge q4)
		temp8 = ~temp8;
		

endmodule
```

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

## Blocking statement (example)
```
module blocking();
	integer clr;
	initial 
		begin
		clr <= #5 0;
		clr <= #4 1;
		clr <= #10 0;
		end
endmodule
```

## Function (example)
```
module fxnn(yout1,yout2,in1,in2);
	output reg [1:0]yout1,yout2;
	input [1:0]in1,in2;
	reg [1:0]temp1,temp2;
	always @(in1,in2)
	begin
	yout1 = annd(in1,in2);
	yout2 = orr(in1,in2);
	end
function annd(input [1:0]a,b);
	reg [1:0]ans;
	begin
		ans = a & b;
	annd = ans;
	end
endfunction
function orr(input [1:0]a,b);
	reg[1:0]ans2;
	begin
		ans2 = a | b;
	orr = ans2;
	end
endfunction
endmodule
```

## Task and Function(example)
```
module and_or(yout1,yout2,in1,in2);
	output [1:0]yout1,yout2;
	input [1:0]in1,in2;
	reg [1:0]temp1,temp2;
	always @(in1,in2)
	my_task(temp1,temp2,in1,in2);
	assign yout1=temp1;
	assign yout2= temp2;
	task my_task(output [1:0]or1,and1,input [3:0]a_in,b_in);
	begin 
		or1= a_in | b_in ;
		and1 = a_in & b_in;
	end
	endtask
endmodule
```

## Parallel to Sequence
```
module para_seque();
	integer dry,cun,jab,exe,dop,gos,pas,box,zoom,bax;
	always 
	begin :seq_a
		#4 dry = 5;
	fork :parallel_a
		#6 cun = 7;
		begin:seq_b
			exe = box;
			#5 jab = exe;
		end
		#2 dop = 3;
		#4 gos = 2;
		#89 pas = 4;
	join
	#8 bax = 1;
	#2 zoom = 52;
	end
endmodule
```

## 3 bit state machine
```
module ctr(clk,rst,y);
	input clk,rst;
	output [3:0]y;
	reg [3:0]ps,ns;
	parameter s0=3'd0,s1=3'd1,s2=3'd2,s3=3'd3,s4=3'd4,s5=3'd5,s6=3'd6,s7=3'd7;
	always @(posedge clk ,posedge rst)
		if (rst)
			begin
				ps = s0;
				ns = s0;
			end
		else
			case(ps)
			s0 : ns = s1;
			s1 : ns = s2;
			s2 : ns = s3;
			s3 : ns = s4;
			s4 : ns = s5;
			s5 : ns = s6;
			s6 : ns = s7;
			default : ns = s0;
			endcase
		always @(ns)
		ps = ns;
	assign y = ps;
endmodule
```

## 1024x8 Memory
```
module memorydes(addr,din,dout,rd,wr,en,clk);
	input [7:0]din;
	input [9:0]addr;
	input rd,wr,en,clk;
	output reg [7:0]dout;
	reg [7:0] mem [1023:0];
	always @ (posedge clk ,posedge en)
	if (en)
		if(rd)
		dout = mem[addr];
	always @(negedge clk , posedge en)
	if (en)
		if (wr)
		mem[addr] = din;
endmodule 
```

# Author
[**Mohammad Mudakir Fazili**](https://www.linkedin.com/in/mudakirfazili14/), *M.Tech Micro-electronics*, NIT Srinagar                                                                                           
*mudakirfazili@gmail.com*


