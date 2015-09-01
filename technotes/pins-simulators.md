Pins Simulators allow you to build and test your Kinoma Create application entirely on your computer by simulating the inputs and outputs of one or more hardware modules.

System debugging is made easier by isolating application software bugs from hardware component and wiring issues.

Rapid development cycles are enabled since you are not required to launch and test on a physical device after every change.

Being able to work on your app away from the hardware makes scenarios such as working on a distributed team, working while traveling, and testing ideas prior to purchasing components all possible.

***
##What they are
BLL’s are JavaScript modules which interact with hardware components such as sensors, buttons and motors. They provide a KinomaJS message API to your application so it can use these types of components.

Two versions of a BLL which support the same API may be created, one for the device which handles the real hardware protocol, and one for the simulator.

Pins Simulators provide the user interface portion of a simulator BLL.

Here we see the Analog Starter sample application loaded into Kinoma Studio. The device folder contains the device BLL analog.js, and the simulator folder contains the simulator BLL analog.xml. Placing them into these folders allows the appropriate one to be loaded at runtime.

![](../images/pinsSimulators/device-and-simulator-folders.png)

The Analog Starter application requires the analog BLL in main.xml when it configures the pins:

	application.invoke( new MessageWithObject( "pins:configure", {
		analogSensor: {
			require: "analog",
			pins: {
				analog: { pin: 52 }
			}
		}
	}));
	
As with any module the implementation may be in pure JavaScript or in the XML syntax, only the root name is specified when the BLL is required. (Here the device BLL is JavaScript and the simulator XML)

***
##Using Them
If your project uses hardware modules which already have existing BLL’s then you may simply place them into the device and simulator folders.

Many BLL’s have already been written and more are being created all the time. With the help of the community we are building up a catalog. You may find reusable BLL’s in the sample applications.

When in use Pins Simulators will show up in the "Hardware Pins Simulator" column on the left in the Kinoma Create Simulator.

Pins Simulators come in two flavors: Data Driven and Custom.

***
##Data Driven Pins Simulators
These are quite easy to create since you don’t need to do any UI design or implementation, instead you define the number and type of data inputs and outputs.

The data axis types supported are float and boolean and they may be configured as either an input or an output.

A **boolean input** axis is appropriate for any digital input such as a digital push button. You select between either a square wave with adjustable frequency:

![](../images/pinsSimulators/digital-input-axis-square-wave.png)

Using the popup menu:

![](../images/pinsSimulators/digital-input-popup.png)

you may select a manual switch button.

![](../images/pinsSimulators/digital-input-manual-switch.png)

A **float input** axis is appropriate for for any analog input such as a pulse sensor or three could be used for an RGB color sensor or an XYZ accelerometer. You select between either a sine, triangle, or square wave with adjustable range and frequency:

![](../images/pinsSimulators/float-input-waveform.png)

Using the popup menu:

![](../images/pinsSimulators/float-input-popup.png)

you may select a manual slider control:

![](../images/pinsSimulators/float-input-slider.png)

A **boolean output** axis is appropriate for any digital output such as a simple LED.

![](../images/pinsSimulators/boolean-output.png)

A **float output** axis is appropriate for any PWM output such as a variable speed DC motor controller, or three could be used for a TriColor LED.

![](../images/pinsSimulators/analog-output.png)

Click and drag your mouse on the output graphs to pause them and examine each sample value.

![](../images/pinsSimulators/pins-simulator-finger-data.png)

***
##Creating Data Driven Pins Simulators
The Analog Drawing Toy sample application uses an accelerometer to detect a physical "shake" gesture, and two potentiometers to control the horizontal and vertical position of the virtual "pen".

An accelerometer BLL handles the X, Y, and Z inputs of the accelerometer, and a single potentiometers BLL handles both of the potentiometer inputs.

![](../images/pinsSimulators/Analog-Drawing-Toy.png)

Both of the BLL’s build Data Driven Pins Simulators. Let’s take a look at `potentiometers.xml`. Below is the entire source, we’ll break it down part by part.

	var PinsSimulators = require('PinsSimulators');

	exports.configure = function(configuration) {
		this.pinsSimulator = shell.delegate("addSimulatorPart", {
			header : { 
				label : "Potentiomers", 
				name : "Analog Toy", 
				iconVariant : PinsSimulators.SENSOR_KNOB 
			},
			axes : [
				new PinsSimulators.AnalogInputAxisDescription(
					{
						valueLabel : "X",
						valueID : "xPos",
						speed : 0.5
					}
				),
				new PinsSimulators.AnalogInputAxisDescription(
					{
						valueLabel : "Y",
						valueID : "yPos",
            			speed : 0.5
          			}
        		),
      		]
    	});
	}

	exports.close = function() {
		shell.delegate("removeSimulatorPart", this.pinsSimulator);
	}

	exports.read = function() {
		return this.pinsSimulator.delegate("getValue");
	}

	exports.pins = {
		xPos: { type: "A2D" },
		yPos: { type: "A2D" }
	};
	
At the top we `require` the PinsSimulators module.

	var PinsSimulators = require('PinsSimulators');
	
This supplies various constants for identifying icons and default controls, and constructors for creating data driven axis descriptions. It also contains the code that implements the data driven pins simulators.

The `configure` function is where most of the action happens.

	exports.configure = function(configuration) {
		this.pinsSimulator = shell.delegate("addSimulatorPart", {
		
We create a JSON structure describing the header and all the data axes we need and pass that to addSimulatorPart which creates our data driven Pins Simulator.

The **header** property contains a label property which is the large red text at top, and a name property which is the smaller gray text below.

	header : { 
		label : "Potentiomers", 
		name : "Analog Toy", 
		iconVariant : PinsSimulators.SENSOR_KNOB 
	},
	
![](../images/pinsSimulators/Potentiomers.png)

It also contains an `iconVariant` property. The constants for the current options are:

|||
|---|---|
|![](../images/pinsSimulators/sensor_button.png) | SENSOR_BUTTON|
|![](../images/pinsSimulators/sensor_dial.png) | SENSOR_KNOB|
|![](../images/pinsSimulators/sensor_led.png) | SENSOR_LED|
|![](../images/pinsSimulators/sensor_slider.png) | SENSOR_SLIDER|
|![](../images/pinsSimulators/sensor_module.png) | SENSOR_MODULE|
|![](../images/pinsSimulators/sensor_guage.png) | SENSOR_GUAGE|

The `axes` property is an array of `AxisDescription` objects.

	axes : [
	
The `AxisDescription` class contains the following properties used to describe a single data axis:

`ioType`

The I/O direction, either ‘input’ or ‘output’.

`dataType`

The data type. Currently the options are ‘float’ or ‘boolean’.

`valueLabel`

The display name of the axis.

`valueID`

This ones important, it’s the name of the property that contains the value for this axis. This is used with `setValue()` and `getValue()`. For example if the valueID for the x axis is "xPos" and the `valueID` for the y axis is "yPos" then the JSON looks like:

* `{ xPos : 0.7408, yPos : 0.7003 }`

`minValue`

The minimum value for the RangeSlider for input axes. The default is zero.

`maxValue`

The maximum value for the RangeSlider for input axes. The default is one.

`value`

The initial value for input axes. Useful for the manual slider control.

`speed`

The initial frequency for waveform generators such as sine, triangle, and square. This is specified in hertz (cycles per second).

`defaultControl`

This allows you to select the initial control from the controls input type popup. The options for **boolean input** axes are:

* `SQUARE_GENERATOR`
* `BUTTON`
 
And the options for **float input** axes are:

* `SINE_GENERATOR`
* `TRIANGLE_GENERATOR`
* `SQUARE_GENERATOR`
* `SLIDER`
  
There are four subclasses of `AxisDescription` which override some of the above properties and are often used since they are more clear and concise. For example the potentiometers example uses two `AnalogAxisInputDescription` objects which already have their `ioType` property set to ‘input’ and their `dataType` property set to ‘float’.

	new PinsSimulators.AnalogInputAxisDescription(
		{
			valueLabel : "X",
			valueID : "xPos",
			speed : 0.5
		}
	),
	
Four useful `AxisDescription` subclasses in PinSimulators.xml are:

* `DigitalInputAxisDescription`
* `DigitalOutputAxisDescription`
* `AnalogInputAxisDescription`
* `AnalogOutputAxisDescription`
  
The `close` function is called when the application shuts down, it’s important to remove the simulator using `removeSimulatorPart`.

	exports.close = function() {
		shell.delegate("removeSimulatorPart", this.pinsSimulator);
	}
	
Next we see the implementation of the BLL API. In this case it is simply a `read` function which returns the current value.

	exports.read = function() {
		return this.pinsSimulator.delegate("getValue");
	}

As noted, this value this value will be a JSON object with `xPos` and `yPos` properties. If the axis were instead an output axis, we could set the x and y values by using the `setValue(value)` function, this could look like:

	exports.write = function() {
		// just an example of using setValue() because the example has no output axes
		var myValue = { xPos : 100, yPos : 100 };
		this.pinsSimulator.delegate("setValue", myValue);
	}

Finally, the pins need to be configured. This should match the configuration done by the device BLL.

	exports.pins = {
		xPos: { type: "A2D" },
		yPos: { type: "A2D" }
	};
	
***
##Custom Pins Simulators

Custom simulators are fun because they allow you to simulate a hardware module or modules in a more physical manner. If you know how to create a User Interface in a Kinoma Create application, then you already know how to build custom pins simulators: they are just KPR containers.

This is an example of a simulator for an LCD output display.

![](../images/pinsSimulators/LCD-output-display.png)

Here we have a simulator that allows for testing the Hover, simulating in-air swipe gestures and physical taps with software buttons:

![](../images/pinsSimulators/hover-customsimulator.png)

***
##Creating Custom Pins Simulators
Custom simulators allow you to dream up and build any interface that you like. The idea is fairly simple: you create a custom User interface for the BLL API. This custom interface is inserted below the standard header for your Pins Simulator.

We’ll see that the structure is very similar to the data driven version, the main difference being that instead of defining an array of data driven axes, we define a behavior which adds our custom interface. The custom interface is responsible for handling the input and output requirements of the BLL’s API.

Below is the source for the i2C Hover sample’s simulator BLL, we’ll again break it down part by part.

	var THEME = require ("themes/flat/theme");
	var CONTROL = require ("mobile/control");
	var PinsSimulators = require ("PinsSimulators");
	var buttonStyle = new Style({ font:"bold 20px", color:["white","white","black"], horizontal:"center" });
	var OrientationBehavior = function(column, data) {
		Behavior.call(this, column, data);
	}
	OrientationBehavior.prototype = Object.create(Behavior.prototype, {
		onCreate: { value: function(column, data) {
			column.partContentsContainer.add(new OrientationLine(data)); 
		}},
	});
	var OrientationButton = Container.template(function($) { return {
		width:80, height:30, active:true, skin:THEME.buttonSkin,
		behavior: Object.create(CONTROL.ButtonBehavior.prototype, {
			onCreate: { value: function(container, $) {
				CONTROL.ButtonBehavior.prototype.onCreate.call(this, container, $.data);
				this.value = $.value;
			}},
			onTap: { value: function(container) {
				this.data.value = this.value;
			}},
		}),
    	contents: [
			Label($, { top:0, bottom:0, style:buttonStyle, string:$.string }),
		]
	}});
    
	var OrientationLine = Container.template(function($) { return {
		left:0, right:0, height:260,
		contents: [
			Label($, { left:0, right:0, top:0, height:30, style:THEME.labeledButtonStyle, string:"Touch" }),
			Container(null, {
				left:0, right:0, top:30, height:110,
				contents: [
					OrientationButton({ data:$, string:"Top", value: "touch top" }, { top:0 }),
					OrientationButton({ data:$, string:"Left", value: "touch left" }, { left:0 }),
					OrientationButton({ data:$, string:"Center", value: "touch center" }, { }),
					OrientationButton({ data:$, string:"Bottom", value: "touch bottom" }, { bottom:0 }),
					OrientationButton({ data:$, string:"Right", value: "touch right" }, { right:0 }),
				],
    		}),
    		Label($, { left:0, right:0, top:150, height:30, style:THEME.labeledButtonStyle, string:"Swipe" }),
			Container(null, {
      			left:0, right:0, top:180, height:70,
      			contents: [
					OrientationButton({ data:$, string:"Up", value: "swipe up" }, { top:0 }),
					OrientationButton({ data:$, string:"Left", value: "swipe to left" }, { left:0 }),
					OrientationButton({ data:$, string:"Down", value: "swipe down" }, { bottom:0 }),
					OrientationButton({ data:$, string:"Right", value: "swipe to right" }, { right:0 }),
				],
			}),
		],
	}});
    
	exports.pins = {
		ts: {type: "Digital", direction: "input"},
		reset: {type: "Digital", direction: "output"},
		data: {type: "I2C", address: 0x42},
	}
	
	exports.configure = function(configuration) {
		this.data = {
			id: 'HOVER',
			behavior: OrientationBehavior,
			header : { 
				label : this.id, 
				name : "HOVER", 
				iconVariant : PinsSimulators.SENSOR_KNOB 
			},
			value: undefined
		};
		this.container = shell.delegate("addSimulatorPart", this.data);
	}
	exports.close = function() {
		shell.delegate("removeSimulatorPart", this.container);
	}
	exports.read = function() {
		var value = this.data.value;
		this.data.value = undefined;
		return value;
	}
	
We must `require` the PinsSimulator module. Additionally we create a button `Style` and `require` a theme and the `mobile/control` library so we can create buttons.

	var THEME = require ("themes/flat/theme");
	var CONTROL = require ("mobile/control");
	var PinsSimulators = require ("PinsSimulators");
	var buttonStyle = new Style({ font:"bold 20px", color:["white","white","black"], horizontal:"center" });
	
The `configure` function serves the same purpose as in the data driven example, to instantiate our pins simulator. The header property is exactly the same as before. The key difference is that instead of an axes array we supply a behavior called `OrientationBehavior` which will instantiate our custom interface:

	this.data = {
		id: 'HOVER',
		behavior: OrientationBehavior,
		
The implementation of `OrientationBehavior` is a single line which adds an instance of our `OrientationLine` into the `partContentsContainer` so it will appear below the standard header.

	OrientationBehavior.prototype = Object.create(Behavior.prototype, {
		onCreate: { value: function(column, data) {
			column.partContentsContainer.add(new OrientationLine(data)); 
		}},
	});

We won’t go into too much detail about the custom UI itself as building a KPR user interface is explained elsewhere, but quickly, the `OrientationLine` contains the labels and containers full of `OrientationButton` objects. Of interest is the value of the buttons such as "swipe up" and "swipe to left".

	OrientationButton({ data:$, string:"Up", value: "swipe up" }, { top:0 }),
	OrientationButton({ data:$, string:"Left", value: "swipe to left" }, { left:0 }),
	
The `onTap` method of the `OrientationButton` sets the data’s value property to it’s own value such as “swipe up” allowing the last button’s vale to be returned by `read()`.

	onTap: { value: function(container) {
		this.data.value = this.value;
    }},
    
The BLL's API consists of a single `read` function, and it returns the last button value pressed.

	exports.read = function() {
		var value = this.data.value;
		this.data.value = undefined;
		return value;
	}
	
Again we implement `close()` and remove the pins simulator.

	exports.close = function() {
		shell.delegate("removeSimulatorPart", this.container);
	}

The BLL's API consists of a single `read` function, and it returns the last button value pressed.

	exports.read = function() {
		var value = this.data.value;
		this.data.value = undefined;
		return value;
	}
	
And we define the `pins` in the same manner as the device BLL does.

	exports.pins = {
		ts: {type: "Digital", direction: "input"},
		reset: {type: "Digital", direction: "output"},
		data: {type: "I2C", address: 0x42},
	}
	
***
##Build a Data Driven Pins Simulator
The Tri-Color LED sample app has a custom pins simulator which displays a colored LED. The goal of this exercise is to create a data driven version of the pins simulator that allows for examining each color component’s value.

![](../images/pinsSimulators/data%20-driven-Tri-Color-LED.png)

One possible solution is shown below:

	var PinsSimulators = require('PinsSimulators');

	exports.configure = exports.configure = function(configuration) {
		this.pinsSimulator = shell.delegate("addSimulatorPart", {
			header : { 
				label : "Tri Color LED", 
				name : "Three PWM outputs", 
				iconVariant : PinsSimulators.SENSOR_LED
			},
			axes : [
				new PinsSimulators.AnalogOutputAxisDescription(
					{
						valueLabel : "Red",
						valueID : "r"
					},
				),
				new PinsSimulators.AnalogOutputAxisDescription(
					{
						valueLabel : "Green",
						valueID : "g"
					},
				),
				new PinsSimulators.AnalogOutputAxisDescription(
					{
						valueLabel : "Blue",
						valueID : "b"
					},
				),
			]
		});
	}

	exports.close = exports.close = function() {
		shell.delegate("removeSimulatorPart", this.pinsSimulator);
	}

	exports.write = exports.write = function(parameters) {
		switch( parameters.color ) {
			case( "red" ):
				this.pinsSimulator.delegate("setValue", "r", parameters.value);
				break;
    		case( "green" ):
      			this.pinsSimulator.delegate("setValue", "g", parameters.value);
      			break;
    		case( "blue" ):
     			this.pinsSimulator.delegate("setValue", "b", parameters.value);
      			break;
		}
	}

	exports.pins = {
		red: { type: "PWM", value: 1 },
		green: { type: "PWM", value: 1  },
		blue: { type: "PWM", value: 1  },
		anode: { type: "Digital", direction: "output", value: 1  } 
	};
	
***
##Build a Custom “Shake” Pins Simulator

![](../images/pinsSimulators/shake-simulator.png)

The Analog Drawing Toy sample app only uses it’s accelerometer to detect a physical shake gesture. It considers a shake gesture to have happened if the current frames x, y, and z values are all a bit different from the last frames.

You can play around with the data driven version and make this happen, but it would be cooler to have a “Shake” button that you just press to simulate a shake.

Hint: an easy way to simulate a shake would be to increment the `x`, `y`, and `z` values returned by a small amount each time the `read()` function is called.

One possible solution is shown below:

	var PinsSimulators = require('PinsSimulators');
	
	var ShakeBehavior = function(content, data, dictionary) {
		Behavior.call(this, content, data, dictionary);
	}
	ShakeBehavior.prototype = Object.create(Behavior.prototype, {
		onCreate: { value: 
    		function(column, data) {
      			// create and add custom shake button
      			buttonContainer = new ButtonContainer(data);
      			column.partContentsContainer.add(buttonContainer);
    		}
		}
	});

	// custom shake button
	var twoColorSkin = exports.twoColorSkin = new Skin({ fill: ['green', 'yellow'], });
	var labelStyle = exports.labelStyle = new Style({ color: 'white', font: '20px', horizontal: 'null', vertical: 'null', });
	var ButtonContainer = exports.ButtonContainer = Container.template(function($) { return { left: 0, right: 0, top: 0, height: 100, contents: [
		Container($, { width: 180, height: 40, active: true, skin: twoColorSkin, name: 'button', behavior: Object.create(ButtonContainerBehavior.prototype), contents: [
			Label($, { left: 0, right: 0, top: 0, bottom: 0, style: labelStyle, string: 'Shake To Clear', }),
		]})
	]}});

	var ButtonContainerBehavior = function(content, data, dictionary) {
		Behavior.call(this, content, data, dictionary);
	}
	ButtonContainerBehavior.prototype = Object.create(Behavior.prototype, {
		onCreate: { value: 
			function(container, data) {
				this.value = { x : 0.1, y : 0.1, z : 0.1 }
			},
		},
		onTouchBegan: { value: 
			function(container, id, x, y, ticks) {
				container.state = 1;
				this.value.x += .01;
				this.value.y += .01;
				this.value.z += .01;
			},
		},
		onTouchEnded: { value: 
			function(container, id, x, y, ticks) {
				container.state = 0;
			},
		},
		getValue: { value: 
			function(container) {
				return this.value;
			},
		},
	});

	// BLL API
	var configure = exports.configure = function(configuration) {
		var data = {
			behavior: ShakeBehavior,			
			header : { 
				label : "Accelerometer", 		
				name : "Simulated Shake", 		
				iconVariant : PinsSimulators.SENSOR_BUTTON 
			}
		};
		this.pinsSimulator = shell.delegate("addSimulatorPart", data);
		value = 0;
	}

	var close = exports.close = function() {
		shell.delegate("removeSimulatorPart", this.pinsSimulator);
	}

	var read = exports.read = function() {
		return buttonContainer.button.delegate("getValue");
	}

	exports.pins = {
		x: { type: "A2D" },
		y: { type: "A2D" },
		z: { type: "A2D" }
	};
