```verilog
`timescale 1ps/1ps
module tb_comparator_N_bit;
  parameter N = 4;
  
  reg [N-1:0] a,b;
  reg exp_gt ,exp_eq ,exp_less;
  wire gt ,eq ,less ;
  integer i ,j ,err = 0;
  
  comparator_4_bit #(N) DUT ( a ,b ,gt ,eq ,less);
  
  initial begin
    $monitor("time=%0t, a=%b, b=%b ,gt=%b ,eq=%b ,less=%b ,exp_gt=%b ,exp_eq=%b ,exp_less=%b",$time ,a ,b ,gt ,eq ,less ,exp_gt ,exp_eq ,exp_less);
    
    
    for (i =0 ; i < 2**N ; i=i+1) begin
      for(j=0 ; j < 2**N ; j=j+1) begin
        a = i;
        b = j;
        exp_gt = 0 ;
   	    exp_eq = 0 ;
        exp_less = 0;
        
        if ( a > b) begin
          exp_gt = 1'b1;
        end else if ( a == b) begin
          exp_eq = 1'b1;
        end else if (a < b) begin
          exp_less = 1'b1;
        end
      #1
      check(gt ,eq ,less ,exp_gt ,exp_eq ,exp_less);
      end
    end
    
    if(err == 0) begin
      $display("---------");
      $display("Test Pass");
      $display("---------");
    end else begin
      $display("---------");
      $display("Test Fail with %d error",err);
      $display("---------");
    end
    
    $finish;
  end
  
  task check( input gt, input eq, input less, input exp_gt, input exp_eq, input exp_less);
    begin
      if( gt !== exp_gt || eq !== exp_eq || less !== exp_less )begin
        $display("[CHECK] ERROR");
        err = err+1;
      end else begin
        $display("[CHECK] MATCHING");
      end
    end
  endtask
  
  initial begin 
    $dumpfile("dump.vcd");
    $dumpvars;
  end
endmodule
