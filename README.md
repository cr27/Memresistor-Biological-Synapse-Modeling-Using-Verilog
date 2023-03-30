# Neuromorphic-Artificial-Neural-Network-in-Verilog -   Modeled After Research Paper - Link Below
Ntinas, Vasileios, et al. “Experimental Study of Artificial Neural Networks Using a Digital Memristor Simulator.” IEEE Transactions on Neural Networks and Learning Systems, vol. 29, no. 10, 2018, pp. 5098–5110., https://doi.org/10.1109/tnnls.2018.2791458.

Work in Progress:

module memresistor(input [1:0] V_te, input [1:0] V_be, input Valid_V_te, input Valid_V_be, input write_enable, input reset, output reg G);
  parameter G_init = 0.5; // initial conductance value
  
  reg G_prev; // previous conductance value
  reg G_new; // new conductance value
  
  always @(posedge write_enable or posedge reset) begin
    if (reset) begin
      G_prev <= G_init; // initialize conductance value
    end else begin
      G_prev <= G_new; // update previous conductance value
    end
  end
  
  always @(*) begin
    G_new = G_prev;
    
    // Update conductance based on voltage inputs
    if (Valid_V_te) begin
      G_new = G_new + V_te[0];
    end
    if (Valid_V_be) begin
      G_new = G_new - V_be[0];
    end
    if (Valid_V_te && V_te[1] > 0) begin
      G_new = G_new + V_te[1];
    end
    if (Valid_V_be && V_be[1] > 0) begin
      G_new = G_new - V_be[1];
    end
    
    // Limit conductance to within 0 and 1
    if (G_new > 1) begin
      G_new = 1;
    end
    if (G_new < 0) begin
      G_new = 0;
    end
  end
  
  // Output conductance value
  assign G = G_new;
endmodule
