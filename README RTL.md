```verilog
module comparator_N_bit #( parameter N = 4) (input wire [N-1:0]a,b,
                                            output wire gt , eq ,less);
  
  assign gt = (a > b);
  assign eq = (a == b);
  assign less = (a < b);
  
endmodule
