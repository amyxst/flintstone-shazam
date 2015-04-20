module SevenSegment(ssOut, nIn);
    output reg [0:6] ssOut;
    input [3:0] nIn;
 
    // ssOut format {g, f, e, d, c, b, a}
     
    always @(nIn)
    case (nIn)
    4'h0: ssOut = ~7'b0111111;
    4'h1: ssOut = ~7'b0000110;
    4'h2: ssOut = ~7'b1011011;
    4'h3: ssOut = ~7'b1001111;
    4'h4: ssOut = ~7'b1100110;
    4'h5: ssOut = ~7'b1101101;
    4'h6: ssOut = ~7'b1111101;
    4'h7: ssOut = ~7'b0000111;
    4'h8: ssOut = ~7'b1111111;
    4'h9: ssOut = ~7'b1100111;
    4'hA: ssOut = ~7'b1110111;
    4'hB: ssOut = ~7'b1111100;
    4'hC: ssOut = ~7'b0111001;
    4'hD: ssOut = ~7'b1011110;
    4'hE: ssOut = ~7'b1111001;
    4'hF: ssOut = ~7'b1110001;
    endcase
endmodule


module DE2_Audio_Parser (
	// Inputs
	CLOCK_50,
	CLOCK_27,
	KEY,

	AUD_ADCDAT,

	// Bidirectionals
	AUD_BCLK,
	AUD_ADCLRCK,
	AUD_DACLRCK,

	I2C_SDAT,

	// Outputs
	AUD_XCK,
	AUD_DACDAT,

	I2C_SCLK,

	SW,
	HEX0,
	HEX1,
	HEX2,
);

/*****************************************************************************
 *                           Parameter Declarations                          *
 *****************************************************************************/


/*****************************************************************************
 *                             Port Declarations                             *
 *****************************************************************************/
// Inputs
input				CLOCK_50;
input				CLOCK_27;
input		[3:0]	KEY;

input		[3:0]	SW;

input				AUD_ADCDAT;

// Bidirectionals
inout				AUD_BCLK;
inout				AUD_ADCLRCK;
inout				AUD_DACLRCK;

inout				I2C_SDAT;

// Outputs
output				AUD_XCK;
output				AUD_DACDAT;

output				I2C_SCLK;
output				[6:0]HEX0;
output				[6:0]HEX1;
output				[6:0]HEX2;
/*****************************************************************************
 *                 Internal Wires and Registers Declarations                 *
 *****************************************************************************/
// Internal Wires
wire				audio_in_available;
wire		[31:0]	left_channel_audio_in;
wire		[31:0]	right_channel_audio_in;
wire				read_audio_in;

wire				audio_out_allowed;
wire		[31:0]	left_channel_audio_out;
wire		[31:0]	right_channel_audio_out;
wire				write_audio_out;

// Internal Registers


reg [18:0] delay_cnt, delay;

reg snd;


// State Machine Registers

/*****************************************************************************
 *                         Finite State Machine(s)                           *
 *****************************************************************************/


/*****************************************************************************
 *                             Sequential Logic                              *
 *****************************************************************************/

 reg ampl = 0;
 reg [31:0] count, noteCount;
 reg [31:0] Prev1, Prev2, Prev3, Prev4;
 reg [31:0] lambda2;
 reg [31:0] c, cs, d, ds, e, f, fs, g, gs, a, as, b;
 reg [31:0] qc, qcs, qd, qds, qe, qf, qfs, qg, qgs, qa, qas, qb;
 reg [31:0] error, checksum, currentNote, prevNote;
 reg [3:0] hexout0, hexout1, hexout2;
 reg [31:0] song1,song2;
 
 
 
initial begin
	Prev1 <= 0;
	Prev2 <= 0;
	Prev3 <= 0;
	Prev4 <= 0;
	count <= 0;
	noteCount <= 32'd0;
	currentNote<= 32'd0;
	prevNote <= 32'd0;
	
	c <= 32'd382219;
	cs <= 32'd360776;
	d <= 32'd340530;
	ds <= 32'd321409;
	e <= 32'd303370;
	f <= 32'd286344;
	fs <= 32'd270278;
	g <= 32'd255102;
	gs <= 32'd240790;
	a <= 32'd227273;
	as <= 32'd214316;
	b <= 32'd202478;
	
	qc <= 32'd0;
	qcs <= 32'd0;
	qd <= 32'd0;
	qds <= 32'd0;
	qe <= 32'd0;
	qf <= 32'd0;
	qfs <= 32'd0;
	qg <= 32'd0;
	qgs <= 32'd0;
	qa <= 32'd0;
	as <= 32'd0;
	qb <= 32'd0;

	error <= 32'd3000;
	checksum <= 32'd0;
	hexout0<=4'd0;
	hexout1<=4'd0;
	hexout2<=4'd0;
	song1 <= ((((((((e^d)^c)^d)^e)^e)^e)^d)^a);
	song2 <= (((((((((((d^e)^g)^e)^d)^e)^d)^c)^e)^d)^a)^g);
	//song1 <= e+d+c+d+e+e+e+d;
	//song2 <= d+e+g+e+d+e+d+c+e+d;
end
	
assign lambda2 = Prev1 + Prev2 + Prev3 + Prev4;
 

always @(posedge CLOCK_50) begin




	if(left_channel_audio_in[31]==ampl) begin

		count <= count + 1;

	end else begin
		Prev4 <= Prev3;
		Prev3 <= Prev2;
		Prev2 <= Prev1;
		Prev1 <= count;
		count <= 0;
		ampl = !ampl;
	end
	
	if((lambda2 < (c + 4000)) && (lambda2 > (c - 4000)))begin
		hexout0 <= 4'd0;
		hexout1 <= 4'd12;
		//currentNote <= c;
		qc <= qc + 1;
	end
	if((lambda2 < (cs + 4000)) && (lambda2 > (cs - 4000)))begin
		hexout0 <= 4'd8;
		hexout1 <= 4'd12;
		//currentNote <= cs;
		qcs <= qcs + 1;
	end
	if((lambda2 < (d + 4000)) && (lambda2 > (d - 4000)))begin
		hexout0 <= 4'd0;
		hexout1 <= 4'd13;
		//currentNote <= d;
		qd <= qd + 1;
	end
	if((lambda2 < (ds + 4000)) && (lambda2 > (ds - 4000)))begin
		hexout0 <= 4'd8;
		hexout1 <= 4'd13;
		//currentNote <= ds;
		qds <= qds + 1;
	end
	if((lambda2 < (e + 4000)) && (lambda2 > (e - 4000)))begin
		hexout0 <= 4'd0;
		hexout1 <= 4'd14;
		//currentNote <= e;
		qe <= qe + 1;
	end
	if((lambda2 < (f + 4000)) && (lambda2 > (f - 4000)))begin
		hexout0 <= 4'd0;
		hexout1 <= 4'd15;
		//currentNote <= f;
		qf <= qf + 1;
	end
	if((lambda2 < (fs + 4000)) && (lambda2 > (fs - 4000)))begin
		hexout0 <= 4'd8;
		hexout1 <= 4'd15;
		//currentNote <= fs;
		qfs <= qfs + 1;
	end
	if((lambda2 < (g + 4000)) && (lambda2 > (g - 4000)))begin
		hexout0 <= 4'd0;
		hexout1 <= 4'd9;
		//currentNote <= g;
		qg <= qg + 1;
	end
	if((lambda2 < (gs + 4000)) && (lambda2 > (gs - 4000)))begin
		hexout0 <= 4'd8;
		hexout1 <= 4'd9;
		//currentNote <= gs;
		qgs <= qgs + 1;
	end
	if((lambda2 < (a + 4000)) && (lambda2 > (a - 4000)))begin
		hexout0 <= 4'd0;
		hexout1 <= 4'd10;
		//currentNote <= a;
		qa <= qa + 1;
	end
	if((lambda2 < (as + 4000)) && (lambda2 > (as - 4000)))begin
		hexout0 <= 4'd8;
		hexout1 <= 4'd10;
		//currentNote <= as;
		qas <= qas + 1;
	end
	if((lambda2 < (b + 4000)) && (lambda2 > (b - 4000)))begin
		hexout0 <= 4'd0;
		hexout1 <= 4'd11;
		//currentNote <= b;
		qb <= b + 1;
	end
	
	noteCount <= noteCount + 1;

	if(noteCount == 32'd50000000)begin
	
		if(qc > 30000000) begin
			//checksum <= checksum + c;
			checksum <= checksum^c;
			currentNote <= c;
		end
		
		if(qcs > 30000000) begin
			//checksum <= checksum + cs;
			checksum <= checksum^cs;
			currentNote <= cs;
		end
		
		if(qd > 30000000) begin
			//checksum <= checksum + d;
			checksum <= checksum^d;
			currentNote <= d;
		end
		
		if(qds > 30000000) begin
			//checksum <= checksum + ds;
			checksum <= checksum^ds;
			currentNote <= d;
		end
		
		if(qe > 30000000) begin
			//checksum <= checksum + e;
			checksum <= checksum^e;
			currentNote <= e;
		end
		
		if(qf > 30000000) begin
			checksum <= checksum^f;
			//checksum <= checksum + f;
			currentNote <= f;
		end
		
		if(qfs > 30000000) begin
			//checksum <= checksum + fs;
			checksum <= checksum^fs;
			currentNote <= fs;
		end
		
		if(qg > 30000000) begin
			checksum <= checksum^g;
			//checksum <= checksum + g;
			currentNote <= g;
		end
		
		if(qgs > 30000000) begin
			checksum <= checksum^gs;
			//checksum <= checksum + gs;
			currentNote <= gs;
		end
		
		if(qa > 30000000) begin
			//checksum <= checksum + a;
			checksum <= checksum^a;
			currentNote <= a;
		end
		
		if(qas > 30000000) begin
			//checksum <= checksum + as;
			checksum <= checksum^as;
			currentNote <= as;
		end
		
		if(qb > 30000000) begin
			checksum <= checksum^b;
			//checksum <= checksum + b;
			currentNote <= b;
		end
		qc <= 32'd0;
		qcs <= 32'd0;
		qd <= 32'd0;
		qds <= 32'd0;
		qe <= 32'd0;
		qf <= 32'd0;
		qfs <= 32'd0;
		qg <= 32'd0;
		qgs <= 32'd0;
		qa <= 32'd0;
		as <= 32'd0;
		qb <= 32'd0;
		noteCount <= 0;
		//hexout2 <= hexout1;
	end
	
	
	
//	if(currentNote == prevNote)begin
//		noteCount <= noteCount + 1;
//	end else begin
//		prevNote <= currentNote;
//		noteCount <= 32'd0;
//	end
	
	if(checksum == song1)begin
		hexout2 <= 4'b1001;
	end
	if(checksum == song2)begin
		hexout2 <= 4'b0010;
	end
	
	
	if(SW[0]==1) begin
		hexout2 <= 4'd0;
		checksum <= 32'd0;
		currentNote <= 32'd0;
		prevNote <= 32'd0;
	end
	
end
	
 SevenSegment(HEX0, hexout0);
 SevenSegment(HEX1, hexout1);
 SevenSegment(HEX2, hexout2);
 assign delay = {SW[3:0], 15'd3000};



wire [31:0] sound = (SW == 0) ? 0 : snd ? 32'd10000000 : -32'd10000000;




assign read_audio_in			= audio_in_available & audio_out_allowed;

assign left_channel_audio_out	= left_channel_audio_in+sound;
assign right_channel_audio_out	= right_channel_audio_in+sound;
assign write_audio_out			= audio_in_available & audio_out_allowed;

/*****************************************************************************
 *                              Internal Modules                             *
 *****************************************************************************/

Audio_Controller Audio_Controller (
	// Inputs
	.CLOCK_50						(CLOCK_50),
	.reset						(~KEY[0]),

	.clear_audio_in_memory		(),
	.read_audio_in				(read_audio_in),
	
	.clear_audio_out_memory		(),
	.left_channel_audio_out		(left_channel_audio_out),
	.right_channel_audio_out	(right_channel_audio_out),
	.write_audio_out			(write_audio_out),

	.AUD_ADCDAT					(AUD_ADCDAT),

	// Bidirectionals
	.AUD_BCLK					(AUD_BCLK),
	.AUD_ADCLRCK				(AUD_ADCLRCK),
	.AUD_DACLRCK				(AUD_DACLRCK),


	// Outputs
	.audio_in_available			(audio_in_available),
	.left_channel_audio_in		(left_channel_audio_in),
	.right_channel_audio_in		(right_channel_audio_in),

	.audio_out_allowed			(audio_out_allowed),

	.AUD_XCK					(AUD_XCK),
	.AUD_DACDAT					(AUD_DACDAT),

);

avconf #(.USE_MIC_INPUT(1)) avc (
	.I2C_SCLK					(I2C_SCLK),
	.I2C_SDAT					(I2C_SDAT),
	.CLOCK_50					(CLOCK_50),
	.reset						(~KEY[0])
);

endmodule

