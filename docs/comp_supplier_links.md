comp selection
define a standard size for components (0603, 0805)
pick something you can solder by hand if its an untested design
less confidant at soldering pick 0805,
more confidant 0603,
cocky 0402

we pick 0603 because its what you will use 90% of the time

this makes it easier to route the design
some components my not fit this size, i.e. bulk decoupling caps

will use 8MHz to make routing slightly easier 

crystal has 12pf load caps

$$
C_{\text{load}} = \frac{C_L^2}{2C_L} + C_{\text{stray}}
$$
this proof is left as an exercise to the reader
$$
2(C_{\text{load}} - C_{\text{stray}}) = C_L 
$$

$$
C_{\text{load}} = 12\text{pf}
$$

$$
C_{\text{stray}} = 5\text{pf}
$$

$$
2(12 - 5) = 2*(7) = 14 = C_L 
$$

only 12pf available and there will be an extra bit a stray floating around so it'll be fine
place a zero ohm on R_EXT and fine tune later if needed, but the value should be some where
around 470R and 1k.

selecting parts normally occurs after you finishes making the schematic, however some part
have to be selected during designing, such as the microcontroller regulator and any other
ic you may have added to the board.

6.3v caps picked for most caps, as its common and gives a good buffer for 5v systems

was going to do alu caps for inrush but there are either big or $$$ so use ceramics

exclude solder jumpers from bill of materials


[digicart link](https://www.digikey.co.nz/short/wt7jmr5z)
