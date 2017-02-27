#This is training Real Time Clock(rtc-3231).
    * The training is not use capemgr.  
    * used module is below.  
![Alt text] (zs-042.jpg)  
    
##Device Tree
I used i2c_2 interface on the BBB.

	/* Pins 19 (SCL) and 20 (SDL) of connector P9 */
	i2c2_pins: pinmux_i2c2_pins {
		pinctrl-single,pins = <
			0x178 (PIN_INPUT_PULLUP | MUX_MODE3)	/* uart1_ctsn.i2c2_sda */
			0x17c (PIN_INPUT_PULLUP | MUX_MODE3)	/* uart1_rtsn.i2c2_scl */
		>;
	};


