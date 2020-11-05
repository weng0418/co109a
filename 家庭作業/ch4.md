#### ALU-nostat

 <img src="jpg/ALU-nostat.jpg" width="1000" height="600"  /> 

* ALU-nostat code
```

/**
 * The ALU (Arithmetic Logic Unit).
 * Computes one of the following functions:
 * x+y, x-y, y-x, 0, 1, -1, x, y, -x, -y, !x, !y,
 * x+1, y+1, x-1, y-1, x&y, x|y on two 16-bit inputs, 
 * according to 6 input bits denoted zx,nx,zy,ny,f,no.
 * In addition, the ALU computes two 1-bit outputs:
 * if the ALU output == 0, zr is set to 1; otherwise zr is set to 0;
 * if the ALU output < 0, ng is set to 1; otherwise ng is set to 0.
 */

// Implementation: the ALU logic manipulates the x and y inputs
// and operates on the resulting values, as follows:
// if (zx == 1) set x = 0        // 16-bit constant
// if (nx == 1) set x = !x       // bitwise not
// if (zy == 1) set y = 0        // 16-bit constant
// if (ny == 1) set y = !y       // bitwise not
// if (f == 1)  set out = x + y  // integer 2's complement addition
// if (f == 0)  set out = x & y  // bitwise and
// if (no == 1) set out = !out   // bitwise not
// if (out == 0) set zr = 1
// if (out < 0) set ng = 1

CHIP ALU {
    IN  
        x[16], y[16],  // 16-bit inputs        
        zx, // zero the x input?
        nx, // negate the x input?
        zy, // zero the y input?
        ny, // negate the y input?
        f,  // compute out = x + y (if 1) or x & y (if 0)
        no; // negate the out output?

    OUT 
        out[16], // 16-bit output
        zr, // 1 if (out == 0), 0 otherwise
        ng; // 1 if (out < 0),  0 otherwise

    PARTS:
   // Put you code here:
   Mux16(a=x,b=false,sel=zx,out=x1);//zx
   Mux16(a=y,b=false,sel=zy,out=y1);//zy
   Not16(in=x1,out=nx1);
   Not16(in=y1,out=ny1);
   Mux16(a=x1,b=nx1,sel=nx,out=x2);//nx
   Mux16(a=y1,b=ny1,sel=ny,out=y2);//ny
   Add16(a=x2,b=y2,out=x2ady2);
   And16(a=x2,b=y2,out=x2any2);
   Mux16(a=x2any2,b=x2ady2,sel=f,out=fo);  
   Not16(in=fo,out=nf);
   Mux16(a=fo,b=nf,sel=no,out=out);
}
```

#### ALU

 <img src="jpg/ALU.jpg" width="1000" height="600"   /> 

* ALU code
```

/**
 * The ALU (Arithmetic Logic Unit).
 * Computes one of the following functions:
 * x+y, x-y, y-x, 0, 1, -1, x, y, -x, -y, !x, !y,
 * x+1, y+1, x-1, y-1, x&y, x|y on two 16-bit inputs, 
 * according to 6 input bits denoted zx,nx,zy,ny,f,no.
 * In addition, the ALU computes two 1-bit outputs:
 * if the ALU output == 0, zr is set to 1; otherwise zr is set to 0;
 * if the ALU output < 0, ng is set to 1; otherwise ng is set to 0.
 */

// Implementation: the ALU logic manipulates the x and y inputs
// and operates on the resulting values, as follows:
// if (zx == 1) set x = 0        // 16-bit constant
// if (nx == 1) set x = !x       // bitwise not
// if (zy == 1) set y = 0        // 16-bit constant
// if (ny == 1) set y = !y       // bitwise not
// if (f == 1)  set out = x + y  // integer 2's complement addition
// if (f == 0)  set out = x & y  // bitwise and
// if (no == 1) set out = !out   // bitwise not
// if (out == 0) set zr = 1
// if (out < 0) set ng = 1

CHIP ALU {
    IN  
        x[16], y[16],  // 16-bit inputs        
        zx, // zero the x input?
        nx, // negate the x input?
        zy, // zero the y input?
        ny, // negate the y input?
        f,  // compute out = x + y (if 1) or x & y (if 0)
        no; // negate the out output?

    OUT 
        out[16], // 16-bit output
        zr, // 1 if (out == 0), 0 otherwise
        ng; // 1 if (out < 0),  0 otherwise

    PARTS:
   // Put you code here:
   Mux16(a=x,b=false,sel=zx,out=x1);//zx
   Mux16(a=y,b=false,sel=zy,out=y1);//zy
   Not16(in=x1,out=nx1);
   Not16(in=y1,out=ny1);
   Mux16(a=x1,b=nx1,sel=nx,out=x2);//nx
   Mux16(a=y1,b=ny1,sel=ny,out=y2);//ny
   Add16(a=x2,b=y2,out=x2ady2);
   And16(a=x2,b=y2,out=x2any2);
   Mux16(a=x2any2,b=x2ady2,sel=f,out=fo);  
   Not16(in=fo,out=nf);
   //Mux16(a=fo,b=nf,sel=no,out=out);//ALU-nostat
