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
    4 - 48 MHz high-speed oscillator with external crystal or ceramic resonator (HSE).
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
   1. button choice
   2. lpf cap 

6. programming
   1. 10 pin stlink
   2. 6 pin plug o'nails

7. usb
   1. micro usb
   2. usb type c
      1. [dropdowns] support circuits
      2. [dropdowns] cc pulldowns

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
