This note provides an overview for how to create a Mobile app in Kinoma Studio.

***
##Creating a New Project
To create a new Kinoma Application, use File->New->Application

![](../images/mobileAppQuickStart/newApplication.jpg)

***
Give your project a name

![](../images/mobileAppQuickStart/projectName.jpg)

***
The next screen allows you to add built-in libraries and your own modules to the project. We'll go over how to do this later in the document, so for now just hit next.

![](../images/mobileAppQuickStart/initialModulePath.jpg)

***
##A Note on XML vs JavaScript in Kinoma Apps
There are two broad approaches to creating Kinoma applications: using an [XML syntax](../../xml/) or working in 100% [JavaScript](../../javascript/). KinomaXML has the advantage of being relatively easy to read and of taking care of a lot of settings for you behind the scenes. But, many people out there are opposed to programming in XML. For these users, Kinoma also offers a pure-JavaScript option in KinomaJS. We will focus on that approach in this document.

To create a JavaScript-only app, simply select "Default KinomaJS Application (JavaScript)" from the final screen of the New Project wizard. Then hit Finish.

![](../images/mobileAppQuickStart/chooseJS.jpg)

***
Applications start with two files (`application.xml` and `main.js`) which you can see in the Project Explorer.

`application.xml` is essentially metadata -- you will rarely need to edit it.

`main.js` is where your application code goes.

![](../images/mobileAppQuickStart/projectExplorerWithScript.jpg)

***
##Containers, Skins, Labels, and Styles
Copy the following code into your main.js and save:

	var greenSkin = new Skin( { fill:"green" } );
	var labelStyle = new Style( { font: "bold 40px", color:"white" } );

	var mainContainer = new Container({
		left:0, right:0, top:0, bottom:0,
		skin: greenSkin,
		contents:[
			new Label({left:0, right:0, height: 40, string: "Hello World", style: labelStyle})
		]
	});

	application.add(mainContainer);
	
This is a complete application that draws a green screen with "Hello World" in white in the center of the screen. You can run it by opening up your application.xml, which will open this screen:

Click the "change simulator" button to pick a good simulator. Since our focus in this tutorial is on mobile applications, I went with the iPhone 4 simulator.

![](../images/mobileAppQuickStart/applicaiton1.jpg)

You can then run your application by hitting the "Run" button. This will launch the Kinoma Simulator running your app:

![](../images/mobileAppQuickStart/helloWorld.jpg)

***
Now, let's unpack what we just built a bit.

In this example, we used a `Skin`, a `Container`, a `Style`, and a `Label`.

####`Skins`
The `Skin` object defines the appearance of content objects. It can fill or stroke content objects with colors or draw or fill content objects with portions of texture objects.

	var greenSkin = new Skin( { fill:"green" } );

Here we only take advantage of the ability to fill content, and choose the color green.

####`Container`
`Containers` are the fundamental building blocks of user interfaces in KinomaJS. At their most basic, they are just rectangles that can be filled with colors or textures. But by adding additional sub-contents, you can create a layout hierarchy that describes complex widgets like buttons, sliders, switches, and tabs. These will eventually build up to make your entire application.

	var mainContainer = new Container({
		left:0, right:0, top:0, bottom:0,
		skin: greenSkin,
		contents:[
			new Label({left:0, right:0, height: 40, string: "Hello World", style: labelStyle})
		]
	});
	
Here we make a `Container` and set a few useful properties. The `left`, `right`, `top`, and `bottom` properties define the margin around the object. If you want it to fill up its entire parent container, you use 0 for each. We also set the skin of the container to be our previously instantiated greenSkin.

The `contents` property of a `Container` (or of any object that inherits from `Container`) is used for defining layout hierarchies. Objects in `contents` will be added to the `Container` in order.

####`Style`
The `Style` object defines the appearance of strings in `label` and `text` objects.

	var labelStyle = new Style( { font: "bold 40px", color:"white" } );
	
Here we give our style a font and a color. The font can be any string that is a legal [CSS font property](http://www.w3schools.com/cssref/pr_font_font.asp). It can specify (in order) font-style, font-variant, font-weight, font-size/line-height, and font-family. The color can be any legal [CSS method of specifying color](http://www.w3schools.com/cssref/css_colors_legal.asp).

####`Label`
The `Label` object is used for displaying text in a UI. They are styled using the `Style` element.

	new Label({left:0, right:0, height: 40, string: "Hello World", style: labelStyle})
	
Here we make our `Label` take up the entire width of the containing content (left:0, right:0), give it a height of 40px (height:40), set its text to "Hello World" and apply the previously instantiated labelStyle.

***
##Grid-Based Layouts & Textures
A common task in user interface design is to layout a collection of widgets in a grid. KinomaJS has two elements to help with this: `Column` and `Line`.

Consider the following `main.js`. It makes use of one large column for the full-app layout, with a central row that has multiple contents. The center-most content uses a `Texture` to draw a png. This png file (in this case "logo.png") must be present in the `src` folder of your project.

	var greenSkin = new Skin({fill:"green"});
	var redSkin = new Skin({fill:"red"});
	var blueSkin = new Skin({fill:"blue"});
	var whiteBorderSkin = new Skin({
		fill:"white", 
		borders:{left:5, right:5, top:5, bottom:5}, 
		stroke:"black"
	});

	var KinomaLogo = new Texture("logo.png");

	var logoSkin = new Skin({
		width:100,
		height:100,
		texture: KinomaLogo,
		fill:"white"
	});

	var mainColumn = new Column({
		left:0, right:0, top:0, bottom:0,
		skin: greenSkin,
		contents:[
			new Line({left:0, right:0, top:0, bottom:0, skin: redSkin}),
			new Line({left:0, right:0, top:0, bottom:0, skin: whiteBorderSkin,
				contents:[
					new Content({left:0, right:0, top:0, bottom:0, skin: whiteBorderSkin}),
					new Content({left:0, right:0, top:0, bottom:0, skin: logoSkin}),
					new Content({left:0, right:0, top:0, bottom:0, skin: whiteBorderSkin}),
				]
			}),
			new Line({left:0, right:0, top:0, bottom:0, skin: blueSkin})
		]
	});

	application.add(mainColumn);
	
When run, this application looks like this:

![](../images/mobileAppQuickStart/grid.jpg)

***
##Basic Interactivity & Events
Every piece of Content in KinomaJS can have an associated Behavior. These behaviors implement the programmatic elements of an application. While behaviors can contain arbitrary methods, each type of content also has a collection of named events that will trigger specific named methods of the content's behavior.

Consider the following application which creates a single Container and listens for touch events on that Container and traces the location of the touch to the console:

	var whiteSkin = new Skin({ fill:"white" });

	var touchBehavior = Object.create(Behavior.prototype, {
		onTouchEnded: {value:  function(container, id, x, y, ticks){
			trace("You touched at: " + x + ", " + y + "\n");
		}}
	});

	var container = new Container({ left:0, right:0, top:0, bottom:0, skin: whiteSkin, behavior: touchBehavior, active: true});

	application.add(container);
	
This example illustrates three important points. First, it shows creating a Behavior and adding a method to it. Second, it demonstrates using the `onTouchEnded` event of a Container. Finally, it demonstrates binding a behavior to content via the Container's constructor.

Two important notes:

1. Containers must be `active` to receive touch events.
2. The content object that triggered the event is passed as an argument to the `onTouchEnded` method. This allows you to use one behavior for many different `Contents` and disambiguate the originating `Content` at event-time.

***
##Using Built-In Widgets
Much of the strength of KinomaJS is in its flexibility. The basic tools of drawing `Containers`, `Labels`, `Skins`, and `Textures` can be combined to make arbitrarily complex widgets and user interfaces. But, for rapid prototyping it is often beneficial to work with pre-existing input widgets. For this, KinomaJS includes a series of built-in libraries.

In this section of the tutorial, we'll walk through some of these widgets one by one. To see many of them all in one place, check out the [controls-buttons](https://github.com/Kinoma/KPR-examples/tree/master/controls-buttons) sample.

***
##Including Built-In Libraries in Your Project
To import a built-in library for your project, start by right-clicking your project folder in the Project Explorer and then selecting Build Path->Configure Build Path...

![](../images/mobileAppQuickStart/buildPath.jpg)

Select the modules tab on Properties screen and then click the "Add Built-in Libraries" button.

![](../images/mobileAppQuickStart/buildProps.jpg)

In this tutorial we'll be using the MobileFramework, Creations, and Controls libraries. So, select those three checkboxes and hit okay.

![](../images/mobileAppQuickStart/selectLibraries.jpg)

***
##A Note on Themes
Many built-in widgets can be customized by theming them. This works by creating a global variable named THEME which can either be a built-in theme (such as the "flat" theme included in the Controls library) or a custom object that provides the necessary properties.

In most of these examples, we will be using the flat theme:

	var THEME = require('themes/flat/theme');
	
***
##Buttons
Basic buttons come from the Controls library. The following example builds a single button that traces to the console when it is tapped:

	var THEME = require('themes/flat/theme');
	var BUTTONS = require('controls/buttons');
 
	var whiteSkin = new Skin({fill:"white"});
	var bigText = new Style({font:"bold 55px", color:"#333333"});

	var MyButtonTemplate = BUTTONS.Button.template(function($){ return{
		top:50, bottom:50, left:50, right:50,
		contents:[
			new Label({left:0, right:0, height:55, string:$.textForLabel, style:bigText})
		],
		behavior: Object.create(BUTTONS.ButtonBehavior.prototype, {
			onTap: { value:  function(button){
				trace("Button was tapped.\n");
			}}
		})
	}});

	var button = new MyButtonTemplate({textForLabel:"Click Me!"});

	var mainContainer = new Container({left:0, right:0, top:0, bottom:0, skin: whiteSkin});
	application.add(mainContainer);
	mainContainer.add(button);
	
When run, you get the following. The screen on the right shows the button while tapped down.

![](../images/mobileAppQuickStart/buttonTap.jpg)

There's a lot going on here, but it all builds on things we've already seen. The big new additions are:

1. We use JavaScript's inheritance mechanism to extend the built-in Button `Container` and `Behavior`.
2. We make our own custom template, rather than just instantiating content directly. This template (`myButtonTemplate`) allows us to make more than one instance that share common properties (in this case, `top`, `bottom`, `left`, `right`, a behavior, and a sub-content `Label`). Templates can be dynamically bound to data at instantiation time. Here we use that feature to set the `string` property of the inner `Label` object.

***
##Checkboxes
Checkboxes are buttons that toggle between a selected and unselected state. This example draws three checkboxes with labels and traces to the console whenever they are selected or unselected. In this sample we add one more syntactical convenience: the ability to specify a `Behavior` definition in-line with the definition of a `Content` template.

	var THEME = require('themes/flat/theme');
	var BUTTONS = require('controls/buttons');

	var whiteSkin = new Skin({fill:"white"});

	var MyCheckBoxTemplate = BUTTONS.LabeledCheckbox.template(function($){ return{
		top:50, bottom:50, left:50, right:50,
		behavior: Object.create(BUTTONS.LabeledCheckboxBehavior.prototype, {
			onSelected: { value:  function(checkBox){
				trace("Checkbox with name " + checkBox.buttonLabel.string + " was selected.\n");
			}},
			onUnselected: { value:  function(checkBox){
				trace("Checkbox with name " + checkBox.buttonLabel.string + " was unselected.\n");
			}}
    	})
	}});

	var checkbox = [];
	checkbox[0] = new MyCheckBoxTemplate({name:"Hello"});
	checkbox[1] = new MyCheckBoxTemplate({name:"World"});
	checkbox[2] = new MyCheckBoxTemplate({name:"Click Me"});

	var mainColumn = new Column({left:0, right:0, top:0, bottom:0, skin: whiteSkin});
	application.add(mainColumn);
	for (var i = 0; i < 3; i++) mainColumn.add(checkbox[i]);
	
![](../images/mobileAppQuickStart/checkBoxes.jpg)

***
##Radio Groups
Radio groups are collections of radio buttons, where only one button can be selected at a time. This sample creates a radio group and traces the name of the selection when it changes:

	var THEME = require('themes/flat/theme');
	var BUTTONS = require('controls/buttons');

	var whiteSkin = new Skin({fill:"white"});

	var MyRadioGroup = BUTTONS.RadioGroup.template(function($){ return{
		top:50, bottom:50, left:50, right:50,
		behavior: Object.create(BUTTONS.RadioGroupBehavior.prototype, {
			onRadioButtonSelected: { value: function(buttonName){
				trace("Radio button with name " + buttonName + " was selected.\n");
		}}})
	}});

	var radioGroup = new MyRadioGroup({ buttonNames: "Lorem,Ipsum,Dolor,Sit,Amet" });

	var mainContainer = new Container({left:0, right:0, top:0, bottom:0, skin: whiteSkin});
	application.add(mainContainer);
	mainContainer.add(radioGroup);
	
![](../images/mobileAppQuickStart/radioButtons.jpg)

***
##Sliders
The Controls library includes basic sliders for inputting (quasi-)continuous numerical values. There are vertical, horizontal, and logarithmic variants. This example demonstrates using a horizontal slider, and traces out the value selected as it is being changed.

	var THEME = require('themes/flat/theme');
	var SLIDERS = require('controls/sliders');

	var greySkin = new Skin({fill:"grey"});

	var MySlider = SLIDERS.HorizontalSlider.template(function($){ return{
		height:50, left:50, right:50,
		behavior: Object.create(SLIDERS.HorizontalSliderBehavior.prototype, {
			onValueChanged: { value: function(container){
			SLIDERS.HorizontalSliderBehavior.prototype.onValueChanged.call(this, container);
			trace("Value is: " + this.data.value + "\n");
		}}})
	}});

	var slider = new MySlider({ min:0, max:100, value:50,  });

	var mainContainer = new Container({left:0, right:0, top:0, bottom:0, skin: greySkin});
	application.add(mainContainer);
	mainContainer.add(slider);
	
![](../images/mobileAppQuickStart/hSlider.jpg)

***
##Switches
Switches are functionally like checkboxes, but with a visual representation of a toggle switch. This example creates an on/off switch and traces to the console when it is toggled.

	var THEME = require('themes/flat/theme');
	var SWITCHES = require('controls/switch');

	var greySkin = new Skin({fill:"grey"});

	var MySwitchTemplate = SWITCHES.SwitchButton.template(function($){ return{
		height:50, width: 100,
		behavior: Object.create(SWITCHES.SwitchButtonBehavior.prototype, {
			onValueChanged: { value: function(container){
				SWITCHES.SwitchButtonBehavior.prototype.onValueChanged.call(this, container);
				trace("Value is: " + this.data.value + "\n");
		}}})
	}});

	var theSwitch = new MySwitchTemplate({ value: 1 });

	var mainContainer = new Container({left:0, right:0, top:0, bottom:0, skin: greySkin});
	application.add(mainContainer);
	mainContainer.add(theSwitch);

![](../images/mobileAppQuickStart/switch.jpg)

***
##Fields
Fields are editable labels, used for capturing small amounts of text. The following example creates an editable label with hint text instructing the user to enter their name. When it is tapped, it brings up the on-device keyboard for input. The keyboard can be closed by tapping on the background container.

This example is quite a bit longer, but it nicely pulls together everything we've learned thus far.

	var THEME = require('themes/sample/theme');
	var CONTROL = require('mobile/control');
	var KEYBOARD = require('mobile/keyboard');

	var nameInputSkin = new Skin({ borders: { left:2, right:2, top:2, bottom:2 }, stroke: 'gray',});
	var fieldStyle = new Style({ color: 'black', font: 'bold 24px', horizontal: 'left', vertical: 'middle', left: 5, right: 5, top: 5, bottom: 5, });
	var fieldHintStyle = new Style({ color: '#aaa', font: '24px', horizontal: 'left', vertical: 'middle', left: 5, right: 5, top: 5, bottom: 5, });
	var whiteSkin = new Skin({fill:"white"});
              
	var MyField = Container.template(function($) { return { 
		width: 250, height: 36, skin: nameInputSkin, contents: [
			Scroller($, { 
				left: 4, right: 4, top: 4, bottom: 4, active: true, 
				behavior: Object.create(CONTROL.FieldScrollerBehavior.prototype), clip: true, contents: [
					Label($, { 
						left: 0, top: 0, bottom: 0, skin: THEME.fieldLabelSkin, style: fieldStyle, anchor: 'NAME',
						editable: true, string: $.name,
							behavior: Object.create( CONTROL.FieldLabelBehavior.prototype, {
								onEdited: { value: function(label){
									var data = this.data;
									data.name = label.string;
              						label.container.hint.visible = ( data.name.length == 0 );							}}
						}),
					}),
					Label($, {
						left:4, right:4, top:4, bottom:4, style:fieldHintStyle, string:"Tap to add name...", name:"hint"
					})
				]
			})
		]
	}});

	var MainContainerTemplate = Container.template(function($) { return {
		left: 0, right: 0, top: 0, bottom: 0, skin: whiteSkin, active: true,
		behavior: Object.create(Container.prototype, {
			onTouchEnded: { value: function(content){
				KEYBOARD.hide();
				content.focus();
			}}
		})
	}});

	var field = new MyField({ name: "" });
	var mainContainer = new MainContainerTemplate();

	application.add(mainContainer);
	mainContainer.add(field);
	
![](../images/mobileAppQuickStart/fields.jpg)

Accept the defaults in the configuration screen and hit next.

![](../images/mobileAppQuickStart/configureApplication.png)

