# article structure

## high level overview

## toc

## intro

## schematic desgin

1. mcu
[hash](e9c2dbe1c46d9b92890eeec189a6750cf9917f85)

### supporting comps

2. decoupling caps
    [datasheet](https://www.st.com/resource/en/datasheet/stm32g431cb.pdf) section 5.1.6 powersupply scheme p.g. 68

    jumper on vbat for optional single cell battery support

    100n x X_VDD pins + 1 x 4.7u = total caps
    4.7u is a bulk decoupling caps

    analog voltage should be mostly stable, therefor a seprate vdda rail isnt needed

    caps should be filtering ceramic capacitors, not eletrolydic

    caps are 6.3v since it larger than the vdd and therefor do not stress the caps or possibly blow them

   1. [dropdown] cap ratings
        X7R for analog
            flatter curve than X5R important to keep analog readings accurate
        X5R for digital
            doesnt need as accurate ratings

        [good article to link to](https://www.kemet.com/en/us/technical-resources/heres-what-makes-mlcc-dielectrics-different.html)


3. crystal
   1. crystal load caps

4. [dropdown] bead selection


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
