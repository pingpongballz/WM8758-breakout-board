# WM8758-breakout-board

Hello! This is a repo documenting my experimentations with making a WM8758 breakout board.

## Quick background and motivation

A friend of mine had a few dead iPod classic 5Gs, and we took out the DAC chip from their mainboards, the venerable WM8758. 
I really enjoyed the sound of the iPod classic 5G, and undertook this project to capture that sound.

## Goal of project

Design and fabricate a breakout board that takes in audio data via the I2S protocol, and outputs audio. Mic functionality is unnecessary. 

## Design considerations

### Modifications to recommended design
Here is the recommended external components by Cirrus Logic [1], page 87
![image](https://github.com/pingpongballz/WM8758-breakout-board/assets/74599812/78729e33-e6f7-4272-86c6-68585e81e20a)

Since mic functionality is not required, Pins 1, 2, 4 and 5 can be left floating. As a consequence, the microphone input stage can be fully omitted.

GPIO 2 and 3 are unneeded, hence can also be left floating. (Page 70 of [1]. Jack detection is not required.)

Pins 21, 22, 23,25 were left floating (I only used CH 1 of the audio output)

Pin 32 can was left floating. (pin 32 is MICBIAS, and unneeded, as mic input was unused)

L1 and L2 are optional; I omitted them. 

Pin 20 was left floating. (LINE_COM unused, only HP_COM used)

Everything else was required. 

### Further considerations
From [1], page 52, the recommended headphone output configuration for common mode rejection of ground noise for CH1 is as belows:
![image](https://github.com/pingpongballz/WM8758-breakout-board/assets/74599812/c1817dc7-b0d8-4672-a37d-36932a24bc1b)
If you want, C1 and C2 can be increased, if you want a lower frequency HPF.

MCLK requires a 12.288MHz clock signal. (If you don't have that, you can always program the PLL in the chip)
The easiest way to get a 12.288MHz signal is to use a crystal oscillator. I got a 3.3V HCMOS oscillator: the ASFL1-12.288MHZ-EC-T from Abracon. [2]
A 0.01uF decoupling capacitor was added for the crystal oscillator. 

## Final Schematic and PCB:

### Schematic
![image](https://github.com/pingpongballz/WM8758-breakout-board/assets/74599812/3807f7eb-5310-447d-ba54-a93f4e4086de)

Quick note: 4.7uF decoupling caps were recommended, but I also found that 1uF caps are also acceptable.

### PCB

![image](https://github.com/pingpongballz/WM8758-breakout-board/assets/74599812/249e4f81-c982-4a94-9ec0-9d5deb4ab958)

![image](https://github.com/pingpongballz/WM8758-breakout-board/assets/74599812/8acaebf6-a710-435b-8617-b6be29c306bd)

Note: Pinout naming for 'SPI' is slightly incorrect. It should be 'I2C/SPI' as the WM8758 also accepts I2C as a control interface (with MODE pin grounded)

Here is another more compact PCB, meant to be used as a raspberry pi pico HAT.
![image](https://github.com/pingpongballz/WM8758-breakout-board/assets/74599812/a1b9c564-09d7-40d7-9647-f2011f827253)

### Programming the WM8758

Pages 78 to 79 of [1] has the entire register map of the WM8758. I won't elaborate on how to program it; if you would like more info on the programming, feel free to contact me.

The WM8758 board was tested on a raspberry pi pico; You can contact me if you want the files.

## References
[1] Cirrus Logic, “Stereo CODEC with Headphone Driver and Line Out” WM8758B datasheet, Dec. 2011 [Revised Jan. 2012].

[2] Abracon LLC, “3.3V HCMOS / TTL COMPATIBLE SMD CRYSTAL CLOCK OSCILLATOR” ASFL-1 datasheet, Apr. 2011.
