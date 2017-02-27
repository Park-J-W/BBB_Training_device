#This is training Real Time Clock(rtc-3231).
    * The training is not use capemgr.  
    * used module is below.  
![Alt text] (zs-042.jpg)  
    
##Device Tree.  
I used i2c_2 interface on the BBB.  
![Alt text] (/adc/XC-4438_microphone/beaglebone-black-pinout.jpg)  

1.add below code to the am335x-bone-common.dtsi :  

	/* Pins 19 (SCL) and 20 (SDL) of connector P9 */
	i2c2_pins: pinmux_i2c2_pins {
		pinctrl-single,pins = <
			0x178 (PIN_INPUT_PULLUP | MUX_MODE3)	/* uart1_ctsn.i2c2_sda */
			0x17c (PIN_INPUT_PULLUP | MUX_MODE3)	/* uart1_rtsn.i2c2_scl */
		>;
	};
  
2.add below code to the am335x-boneblack.dts :  

	&i2c2 {
		status = "okay";
		clock-frequency = <100000>;
		
		pinctrl-0 = <&i2c2_pins>;
		pinctrl-names = "default";
			
		ds3231: ds3231_rtc@68 {
			compatible = "maxim,ds3231";
			reg = <0x68>;
		};	
	};  

3. Device tree compile.
4. $ dtc -I fs /proc/device-tree -> check device tree.
4. i2cdetect -y -r 2 -> check i2c_2 device.

####good reference  
1. [blog.fraggod.net][blog.fraggod.net link] : 
[blog.fraggod.net link]: http://blog.fraggod.net/2015/11/25/replacing-built-in-rtc-with-i2c-battery-backed-one-on-beaglebone-black-from-boot.html

##ds3231 Driver.  
Rtc-3231 driver file is in the driver/rtc/rtc-ds1307.c  
Find the id of ds3231 on the driver file. If found that, don't need to modify or add code.  
If not, add the following line to the ds1307_id table.  
{ "ds3231", ds_3231 },  

###Built-in kernel the driver.  
1. make ARCH=arm menuconfig  
2. enter the Search Configuration Parameter by typing '/'.  
3. search the Real Time Clock by inputing the RTC_DRV_DS1307 on the search box.  
4. input the number corresponding to Real Time Clock.  
5. and then, hit the space bar until the represents to "*".  
6. save the configure, compile, download image and powerup device.  
  
###Module load.  
1. run the above 1~4.  
2. and then, hit the space bar until the represents to "M".  
3. save the configure.  
4. save the configure, compile, download image and powerup device.  

###R.C.T setting and check.  
1. date  
2. rdate -s {tiem server} (i.e time.bora.net ) -> it will set the OS time as the received time from time server.  
3. check the time by using date command.  
3. hwclock -w -> it will set up OS time to the RTC.  
4. check the RTC time by using hwclock command.  
5. reboot  
6. check the RTC time by using hwclock command.  

####good reference  
1. [Adding a Real Time Clock to BeagleBone Black][Adding a Real Time Clock to BeagleBone Black link] : 
[Adding a Real Time Clock to BeagleBone Black link]: https://learn.adafruit.com/adding-a-real-time-clock-to-beaglebone-black/overview   
