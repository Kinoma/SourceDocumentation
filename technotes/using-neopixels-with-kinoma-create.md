
This tech note steps you through the process of using [Adafruit NeoPixels](http://www.adafruit.com/category/168) with Kinoma Create.  This also works for any other Digitally Addressable RGB LED with the WS2812 chip that is compatible with the NeoPixel Library.

![](img/setup.jpg)
***
## Getting Started
Before you start playing with NeoPixels on Kinoma Create projects, make sure you have the necessary tools and parts!  This includes an Arduino board such as the [Arduino UNO](http://store-usa.arduino.cc/products/a000066) or [Adafruit's Pro Trinket](https://www.adafruit.com/products/2010).

![](img/arduinoboards.png)

To set up your NeoPixels and to get started with the Arduino IDE, definitely check out [Adafruit's NeoPixel guide](https://learn.adafruit.com/adafruit-neopixel-uberguide/overview).  It will walk you through how to safely wire up your NeoPixel strip and the basics of using the NeoPixel Library through the Arduino IDE.

If you aren't using too many LED pixels, they can be powered through the 5V output of the Kinoma Create instead of an external battery source.  The Arduino Board can also be powered through this output.
***
## Why Are We Using an Arduino Board?
The NeoPixel strip is driven using a strictly timed series of digital outputs to accurately control each pixel. In order to achieve this effect, we need real-time control of the digital output. Linux-based systems like Kinoma Create have difficulty maintaining this level of precision, due to the inherent variances of the Linux scheduler. A common solution to this problem is to insert a small microcontroller between the Linux-based system and the component that requires real-time control. You see this in many breakout boards that convert sensors that use one-wire or other obscure communication protocols into the more common I2C. Here, we use an Arduino as the microcontroller-in-the-middle because many excellent libraries already exist for controlling NeoPixels with Arduino.
***
## Setting Up Serial Communication
To set up communication between Kinoma Create and the Arduino board, it is necessary to connect the UART TX port on the Kinoma Create to the RX port on your Arduino Board.

![](img/serialconnection.png)

![](img/wiring.jpg)

Configure the TX pin on the Kinoma Create with a Serial BLL and set up your Arduino to look for Serial communications as follows.
***
## Kinoma Create Setup
Set up your **led.js** BLL to take in Serial input, with a baud rate of 9600

	exports.pins = {
		led: {type: "Serial", baud: 9600}
	};

	exports.configure = function(configuration) {
		this.led.init();
	}

	exports.close = function() {
		this.led.close();
	}

	exports.sendCommand  = function (param) {
		var line = param.command + "\r\n";
		this.led.write(line);
	}
Don't forget to configure the BLL in **main.js**!

	application.invoke( new MessageWithObject( "pins:configure", {
		led: {
			require: "led",
			pins: {
				led: {tx: 31}
			}
		},
	}));

When you are ready to send a command to the Arduino, invoke the **sendCommand()** handler with your parameters.

	var param = {command: setting};
	application.invoke(new MessageWithObject("pins:/led/sendCommand", param));
***
## Arduino Set Up
In the setup method of the Arduino code, it is extremely important to begin Serial communication.  Make sure the baud rate matches the baud rate of the Kinoma Create.  In the main loop, check if Serial information is available and pull the information to be used further in the code.

	String inData;
	
	// Setup Code
	void setup() {
		Serial.begin(9600);
	}
	
	// Loop Code
	void loop() {
		// get serial data
		while (Serial.available() > 0) {
			char received = Serial.read();
			inData += received; 
		}
	}

Don't forget to do any other code necessary to initialize and light up your LED strip!
***
## Considerations
If you see visible distortion of color, definitely consider giving the strip an external power source.  This distortion of color happens because there is not enough current to give each individual LED pixel the power it needs.  Another solution would be to lower the brightness in order to reduce the overall power consumption.
![](https://learn.adafruit.com/system/assets/assets/000/010/715/medium800/leds_brownout.jpg)

Lowering the brightness can also help diffuse the light.  The NeoPixels are fairly bright, so we have found it helpful to lower the brightness with the **setBrightness** method and place a diffuser over the LEDs.

Another thing to consider is the effect of delay times within the main loop.  Adding delays allows you to create cool blinking sequences but it also delays the next iteration of the main loop.  This is not a problem if the blinking sequence rarely changes, but if you are expecting constant serial inputs and having the strip react to those inputs, the delay time will postpone the next command until the current iteration of the main loop is finished.  Basically, by adding a delay in the main loop to produce a blinking effect, you are also adding a delay before going to the next serial message.
***
## Resources
*	Tank Robot and Controls Project with NeoPixels
*	[NeoPixel Guide](https://learn.adafruit.com/adafruit-neopixel-uberguide/arduino-library)
*	[Arduino Reference](https://www.arduino.cc/en/Reference/HomePage)