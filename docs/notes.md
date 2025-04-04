# article structure

## high level overview

## toc

## intro

## schematic design

1. mcu
[hash](e9c2dbe1c46d9b92890eeec189a6750cf9917f85)

### supporting comps

2. decoupling caps
    [hash](98fe0d07d26a1c95464953851bdf675cf1a5116f)
    [datasheet](https://www.st.com/resource/en/datasheet/stm32g431cb.pdf) section 5.1.6 power supply scheme p.g. 68

    jumper on vbat for optional single cell battery support

    100n x X_VDD pins + 1 x 4.7u = total caps
    4.7u is a bulk decoupling caps

    analog voltage should be mostly stable, therefor a separate vdda rail isnt needed

    caps should be filtering ceramic capacitors, not electrolytic

    caps are 6.3v since it larger than the vdd and therefor do not stress the caps or possibly blow them

   1. [dropdown] cap ratings
        X7R for analog
            flatter curve than X5R important to keep analog readings accurate
        X5R for digital
            doesnt need as accurate ratings

        [good article to link to](https://www.kemet.com/en/us/technical-resources/heres-what-makes-mlcc-dielectrics-different.html)


3. crystal
    [hash](6430cd6cd9e619b09895460b4084fa307ed90d1f)
    [datasheet](https://www.st.com/resource/en/datasheet/stm32g431cb.pdf)
    section 5.3.7 External clock source characteristics p.g. 100
    4 - 48 MHz high-speed oscillator with external crystal or ceramic resonator (HSE). table 40 in datasheet
    It can supply clock to system PLL. The HSE can also be configured in bypass mode for an external clock.

    4 pad crystal, the 2 extra pads connected to ground are for mounting and well as case grounding for emi protection

   1. crystal load caps
    $$
        C_{\text{load}} = \frac{C_{L1} \times C_{L2}}{C_{L1} + C_{L2}} + C_{\text{stray}}
    $$
    $$
        C_{\text{stray}} \approx5\text{pf}
    $$
    $$
        C_{L1} = C_{L2} 
    $$

    - C_L : external load capacitances
    - R_EXT : external resistor to limit the inverter output current
    - C_load : crystal load
    - C_stray : stray capacitance, sum of the device pins and PCB trace capacitances
    1. [dropdown] extra clock src
       low power clock sources for real time clock (RTC)
       32.768 kHz low-speed oscillator with external crystal (LSE)

    2. [dropdown] feedback values
        [appnote](https://www.st.com/resource/en/application_note/cd00221665-oscillator-design-guide-for-stm8af-al-s-stm32-mcus-and-mpus-stmicroelectronics.pdf)

        | freq | feedback resistor range |
        | ---- | ---------------------- |
        | 32.768 kHz  | 10 to 25 MΩ |
        | 1 MHz | 5 to 10 MΩ|
        | 10 MHz | 1 to 5 MΩ|
        | 20 MHz | 470 kΩ to 5 MΩ|



4. [dropdown] bead selection
    [hash](ed590af96a158ff80046151752b1db2daed66063)
    A bead should not actually be necessary for this circuit its more for demonstration purposes.
    you want a bead to operator within its restive zone
    a bead should not be on digital power supplies due to the switching of downstream circuits causes unnecessary power draw.
    [article on them](https://resources.altium.com/p/how-do-ferrite-beads-work-and-how-do-you-choose-right-one)
    

### input
5. reset
    [hash](ca8ece5d9ce18528ab4d70325f88380fb582dfc1)
    [datasheet](https://www.st.com/resource/en/datasheet/stm32g431cb.pdf)
    section 5.3.15 NRST pin characteristics p.g. 117

    talk about sheet label
   
    the reset button as you know resets the micro controller program, as well as current memory 
    
    there is an internal pull up within the mcu, no need for an external pull up.

    led to show that the reset button has be pressed, is completely optional

   1. button choice
        the best button you can use is the button you have on hand or from a kit, buttons are expensive. make it small

   2. lpf cap
      cap should be place as close to the mcu as possible
      100n as sped'ed by the datasheet

6. programming
    [hash](1d1887cbb8e0dd7f1c3b1ad92e817dd7263f4d1d)
    [programming app note](https://www.st.com/resource/en/application_note/dm00354244-stm32-microcontroller-debug-toolbox-stmicroelectronics.pdf)
    
    swd because its what the cheap stlinks support
    SWO to allow debug printing

    swd has clock, and data

   1. boot button
    [hash](c9936e445c0efe0e008e8a08ca916a944b0e9a81)
    [datasheet](https://stm32-base.org/guides/connecting-your-debugger.html)
    stlink clone

    gnd for running program
    vcc for booting in to programming mode

   2. 10 pin idc stlink
        this will be used as it easy to use with cheap knock off stlink
   3. 6 pin plug o'nails
        can be used instead of stlink but this should only be used when space is a constraint

7. usb
    [hash](fe8af82cef5476babfd91a4354a0ed6b2c7fa221)
    usb type c may sound like just a connector, but it actually provides additional information to what ever device it is plugged in to

    reasons for usb: 
    power
    progamming
    serial output

    0 ohm jumper between shield and gnd

    usb choice

    has internal termination resistors

    power out at a low powered hub is 4.4 V to 5.50 V.

   1. micro usb
    simple to implement, only need the connector
    no one has thing that have usb a ports now and no one own a type c to micro cable
   2. usb type c
      stm32G4 support usb c natively as well as pd
      will use provided pd input on the stm but will also show how to setup usb if these arnt available 

      14 pin version 
      hides unused pins 
      cc_x used for power negotiation

      forgot in hash but cc resistors should be DNP
      
      10uf decoupling per usb spec

      power in should be 5v @ 1.5A to 3A

      1. [dropdowns] cc pulldowns
        [change if you want more current](https://www.microchip.com/content/dam/mchp/documents/OTH/ApplicationNotes/ApplicationNotes/00001953A.pdf)
        5.1k pulldowns
      2. [dropdowns] pd
         [appnote](https://www.st.com/resource/en/user_manual/dm00598101-managing-usb-power-delivery-systems-with-stm32-microcontrollers-stmicroelectronics.pdf)

8. power
    [hash](b36409520eb05e74d2c82bd2b8d01cf1bc5a77b1)
    schematic is getting crowded, introduce hierarchical sheets here

    usb circuit now goes in here because it related to power

    or-ing circuit to allow both usb and swd header to be used at the same time

    min power in = 4.4v
    
    jumper so if the programmer wants it to use the 5v or 3.3v rails as a v sense line it still can
    
    optional leds for power status

    az1117-3.3 because its so ubiquities and provides enough voltage for anything you want to do
    v_drop = 1.2v
    v_out = 3.3v
    v_in_min = 3.3 + 1.2 = 4.5v
    v_drop_diode ~= 0.5v assuming input = 5v
    i will ignore the fact that usb can output voltage that is that low

   2. decoupling caps
        alu caps because bigger bulk decoupling
   3. [dropdown] temp calcs
        work out max current draw and avg current draw
        $$
        P_{\text{draw}} = IV
        $$
        find Thermal Resistance on datasheet
        $$
        \theta_{JA} = \frac{C^{\circ}}{W}
        $$
        $$
            P_{\text{draw}} \theta_{JA} = C^{\circ}_\text{rise} 
        $$

### prefs
9. leds
    [hash](ae1e725e1f196119e2582571466d08307c622540)
    4 led configs

    active low direct drive
    active high direct drive
    use these for basic, debug and status leds

    active low BJT drive
    active high BJT drive
    if you want to use more or higher power leds use these

    1. led resistors
        forward voltage = V_f
        Luminous Intensity vs. Forward Current graph
        $$
            V_f = I_f R
        $$
        $$
            \frac{V_f}{I_f} = R
        $$
        pick a Luminous Intensity and find R

        honestly 90% of the time this doesnt matter, just hoon on a 500ohm and accept the brightness of the led, if you want brighter use a smaller resistor and dimmer a bigger resistor

10. distance sensor
    [hash](8c9ae6b2d3eeb6c376ba404fffb7075c1cead87e)
    analog
    [G4 opamp datasheet](https://www.st.com/resource/en/application_note/dm00605707-operational-amplifier-opamp-usage-in-stm32g4-series-stmicroelectronics.pdf)
    fig 8 p.g. 9
    can use adc normally but connect these to internal opamp to show a more complex opamp configs, allowing for pga to be used and extra gain to be added before going to the adc
    this is a stm32G4 feature and that should be explained. but its good to show that some mcus have extra features
    PA1-3 fits all these requirements
    1. [sharp sensor](https://www.sparkfun.com/infrared-proximity-sensor-sharp-gp2y0a21yk.html)
    
    2. [photoresistor](https://www.sparkfun.com/mini-photocell.html)
    
    digital
    PA8-9 are the i2c2 interface
    optional pullups for use with the i2c sensor, and allowing use for the hc-sr04
    3. [ardunio ultra sonic one / HC-SR04](https://www.sparkfun.com/ultrasonic-distance-sensor-hc-sr04.html)
    4. [i2c sensor](https://www.adafruit.com/product/4742) 

    both interfaces have 3.3v and gnd, digital has select for 5v since the HC-SR04 wants it, pins are 5v tolerance tho

11. pin headers
    [hash](381e4038692ab114b09633142ac0fe56978363f7)
    2.54mm pitch for standard dupont connectors
    expose everything
    added extra headers for gnd and 3.3v

### extras 

12. ESD
    added eds to usb input

13. protection circuits
    down stream 3.3v @ 1a = 3.3w
    v_in = 5v
    p = iv
    3.3w = 5*i
    3.3/5 = i
    


14. test points / jumpers