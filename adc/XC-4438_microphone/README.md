This is an microphone
=============

Reference to
-------------

1. [aduino with XC-4438][aduino with XC-4438 link] : Product Page
[aduino with XC-4438 link]: https://www.jaycar.com.au/audio-matrix-spectrum 

2. [VegetableAvenger BBBIOlib][VegetableAvenger link] : Grate Referrnce
[VegetableAvenger link]: https://github.com/VegetableAvenger/BBBIOlib/tree/master/Demo/Demo_ADC

3. [Analysis XC-4438][Analysis XC-4438 link] : Grate Analysis XC-4438
[Analysis XC-4438 link]: http://www.axino-tech.co.nz/documents/duinotech%20microphone%20sound%20detector.html


![Alt text] (XC-4438.jpg)
  
  
  
###Product Pages Description

####Connections:

BBB : P9_01 -> GND : XC-4438 (G	Ground)  
BBB : P9_03 -> +   : XC-4438 (Power)  
BBB : P9_36 -> A0  : XC-4438 (Sound Signal from Microphone)  
BBB : N/C -> D0 : XC-4438 (Digital Signal from Sound Module)  
  
Note that we are using the VIN pin on the Nano as a 5V supply to the LED Matrix- this will only work if the Nano is receiving 5V from the USB socket, and may burn out the LED's if VIN is much more than 5V. Luckily, there are two GND connections on the Nano board (there is also an extra GND and 5V connection on the ICSP header, if you need them).

Although it looks like a lot of connections, the connections to the matrix are made in the same order at both ends, so the wiring can be made neat by taking a strip of ten jumper leads and running them between the two boards, eight for the connections at one end, then two for the connections at the other end. The connections to the Sound Module can similarly be made with a strip of four jumper leads- just watch out, because they aren't in any obvious order.

####Setup:

The small blue potentiometer on the Sound Module may need to be adjusted. We want the L2 LED to be flickering between on at the slightest sound. If the L2 LED is on, turn the brass screw anticlockwise. If L2 is off, turn the screw clockwise until it turns on, then turn anti-clockwise until it just goes off. The LED should now flash on if the microphone is tapped. This is the only adjustment that needs to be made, and after the code is uploaded, your Audio Matrix Spectrum is complete.

####Code:

The code consists of three files- the main sketch file and two included files. The two extra files perform what is called a 'Fast Fourier Transform', which turns audio samples into a representation of the sound frequencies in that sample. The main sketch mostly revolves around a timer interrupt, which ensures that the display refresh and the audio samples occur at a regular rate. When a complete set of samples has been captured, the Fast Fourier transform is performed, and the results are converted for display on the matrix.

Make sure that all the files in the right folder, choose the Nano board and upload the code. You should immediately see at least a line of LEDs lit at the bottom of the matrix. If some columns are missing, you might have one of the A, B, C or D wires not connected correctly. If there is random flickering, check the other wires to the matrix. The display should respond to a gentle tap on the microphone- if this doesn't happen, check the A7 connection to the Sound Module.

####Improvements:

The easiest way to adjust the sensitivity of the Spectrum Display is to change the distance between the sound source and the microphone, but if you find that’s not enough, you could change this line:

data[i] = a;
to something like:

data[i] = a*4;
to increase the sensitivity. Because we are only dealing with integer data types, we can only multiply by whole numbers.

The D0 connection from the Sound Module has been connected for completeness, but isn’t used in the current sketch. You could use this input to drive a large LED to give another display that moves with the beat.

Sketch:

See the attached .zip with all three files needed for the sketch.
