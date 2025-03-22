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

   1. micro usb
    simple to implement, only need the connector
    no one has thing that have usb a ports now and no one own a type c to micro cable
   2. usb type c
      stm32G4 support usb c natively as well as pd
      will use provided pd input on the stm but will also show how to setup usb if these arnt available 

      14 pin version 
      hides unused pins 
      cc_x used for power negotiation
      
      10uf decoupling per usb spec

      power in should be 5v @ 1.5A to 3A

      1. [dropdowns] cc pulldowns
        [change if you want more current](https://www.microchip.com/content/dam/mchp/documents/OTH/ApplicationNotes/ApplicationNotes/00001953A.pdf)
        5.1k pulldowns
      2. [dropdowns] pd
         [appnote](https://www.st.com/resource/en/user_manual/dm00598101-managing-usb-power-delivery-systems-with-stm32-microcontrollers-stmicroelectronics.pdf)

8. power
   
   
   1. voltage reg
   2. decoupling caps
   3. [dropdown] temp calcs
   4. [dropdown] noise rejection

### prefs
9. leds
    1. led resistors

10. distance sensor
    1. sharp sensor
    2. photoresistor
    3. ardunio ultra sonic one

11. pin headers

12. esd & protection circuits