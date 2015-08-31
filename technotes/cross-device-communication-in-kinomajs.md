This tutorial will teach you how to communicate between multiple devices using KinomaJS. It assumes a basic understanding of how to use Kinoma Studio, how to build grid-based user interfaces with KinomaJS, and knowledge of Messages and Handlers.

If you are not familar with these topics, please refer to the [Mobile Apps in Kinoma Studio Quick Start Guide](../mobile-apps-in-kinoma-studio/) and the [Asynchronous Communication in KinomaJS tutorial](../asynchronous-communication-in-kinomajs/).

***
##Getting Started

Create two new Application Projects with JavaScript main files (main.js) as per [these instructions](http://pact.eecs.berkeley.edu/andy/KinomaTutorial/#newProject). One application should have an ID of tutorial3device.app while the other should have an ID of tutorial3phone.app. That is, the application.xml files for the two applications should look like this:

![](http://kinoma.com/develop/documentation/technotes/images/cross-device-communication-in-kinomaJS/phoneapplicationxml.jpg)

![](http://kinoma.com/develop/documentation/technotes/images/cross-device-communication-in-kinomaJS/deviceapplicationxml.jpg)

To help keep the two applications straight, we will adopt the following conventions in this tutorial:

* Code for the Device application will always be shown with a

<pre><code class="deviceCode">light green background.
</code></pre>

* Code for the Phone application will always be shown with a

<pre><code class="phoneCode">light grey background.
</code></pre>

* Screenshots of the Device application will be taken in the Kinoma Create Simulator.

* Screenshots of the Phone application will be taken in the iPhone 4 simulator.

***
##Discovery Using application.shared

To interact between two devices on the same network, the KinomaJS application running on one of the devices must first obtain the IP address and communications port of the KinomaJS application running on the other. This process is called discovery and is simple to implement in KinomaJS. Consider these two basic (but complete) KinomaJS applications:

<pre><code class="deviceCode">//@program 
 var ApplicationBehavior = Behavior.template({
	onLaunch: function(application) {
	  application.shared = true;
	},
	onQuit: function(application) {
	  application.shared = false;
	},
  })
  
  application.behavior = new ApplicationBehavior();  
</code></pre>

<pre><code class="phoneCode">//@program
  Handler.bind("/discover", Behavior({
	onInvoke: function(handler, message){
	  trace("Found the device.\n");	
	}
  }));
  
  var ApplicationBehavior = Behavior.template({
    onDisplayed: function(application) {
	  application.discover("tutorial3device.app");
	},
	onQuit: function(application) {
	  application.forget("tutorial3device.app");
	},
  })
  
  application.behavior = new ApplicationBehavior();
</code></pre>

The device application is trivially simple, yet surprisingly powerful. By setting `application.shared = true`, we tell the Kinoma platform to make this application `discoverable` by other applications on the network (among other benefits that we will explore later in this tutorial).

The phone application has two major components. First, it defines a `Handler` bound to the path "/discover". This path is called automatically by the Kinoma Platform whenever a relevant device is discovered on the network. Second, it uses `application.discover(id)` to turn on discovery and start searching for application on the network with the specified application ID. In this case, we use "tutorial3device.app" because we know that's what we called the device-side application.

If you run these two programs at the same time, you will see that the phone application traces to the console "Found the device" every time you open and close the device application. (You, of course, will not see anything interesting in the simulators yet, because we haven't built a UI.)

***
##Invoking Handlers Across Devices

Once you have discovered another KinomaJS application on the network, you can invoke its `Handlers` directly using Messages constructed with fully-qualified URLs. This url is provided as part of the message to the "/discover" Handler, as shown in these apps:

<pre><code class="deviceCode">//@program
  Handler.bind("/respond", Behavior({
	onInvoke: function(handler, message){
	  message.responseText = "You found me!";
	  message.status = 200;	
	}
  }));

  var ApplicationBehavior = Behavior.template({
	onLaunch: function(application) {
	  application.shared = true;
	},
	onQuit: function(application) {
	  application.shared = false;
	},
  })

  application.behavior = new ApplicationBehavior();
</code></pre>

<pre><code class="phoneCode">//@program
  Handler.bind("/discover", Behavior({
	onInvoke: function(handler, message){
	  var discovery = JSON.parse(message.requestText);
	  handler.invoke(new Message(discovery.url + "respond"), Message.TEXT);
	},
	onComplete: function(handler, message, text){
	  trace("Response was: " + text + "\n");
	}
  }));

  var ApplicationBehavior = Behavior.template({
	onDisplayed: function(application) {
	  application.discover("tutorial3device.app");
	},
	onQuit: function(application) {
	  application.forget("tutorial3device.app");
	},
  })

  application.behavior = new ApplicationBehavior();
</code></pre>
	
Here, we've modified the device code to add a `Handler` bound to the path "/respond." Within its `onInvoke` function, we set `message.responseText` and `message.status` to communicate back with the invoking application.

On the phone side, we modify the "/discover" `Handler` to invoke a message to the discovered application. We pull the url needed for communication (which includes both an address and a port) from the Message delivered to the Handler. We then add on the path we wish to invoke in the other application ("respond") and invoke the new Message against our Handler, requesting a TEXT response.

We capture and trace that response in the `onComplete` function of the Handler. When both applications are run, the phone app should trace "Response was: You found me!" to the console every time the device app is started.

***
##Building a Basic Cross-Device User Experience
We can combine everything that we've learned in the first two tutorials with this new information in order to build a basic cross-device user experience. **We will be using the Controls library in this example, so be sure to load it into your build path as in [Tutorial 1](../mobile-apps-in-kinoma-studio.php/)**.

Consider the following two applications:

<pre><code class="deviceCode">//@program
  var whiteSkin = new Skin( { fill:"white" } );
  var labelStyle = new Style( { font: "bold 40px", color:"black" } );

  Handler.bind("/getCount", Behavior({
	onInvoke: function(handler, message){
	  count++;
	  counterLabel.string = count;
	  message.responseText = JSON.stringify( { count: count } );
	  message.status = 200;
	}
  }));

  Handler.bind("/reset", Behavior({
	onInvoke: function(handler, message){
	  count = 0;
	  counterLabel.string = "0";
	  message.responseText = JSON.stringify( { count: "0" } );
	  message.status = 200;
	}
  }));

  var counterLabel = new Label({left:0, right:0, height:40, string:"0", style: labelStyle});
  var mainColumn = new Column({
	left: 0, right: 0, top: 0, bottom: 0, skin: whiteSkin,
	contents: [
	  new Label({left:0, right:0, height:40, string:"Counter:", style: labelStyle}),
	  counterLabel,
	]
  });
    
  var ApplicationBehavior = Behavior.template({
	onLaunch: function(application) {
	  application.shared = true;
	},
	onQuit: function(application) {
	  application.shared = false;
	},
  })
    
  count = 0;
  application.add(mainColumn);
  application.behavior = new ApplicationBehavior();
</code></pre>

<pre><code class="phoneCode">//@program
  var THEME = require("themes/flat/theme");
  var BUTTONS = require("controls/buttons");
    
  deviceURL = "";
    
  var whiteSkin = new Skin( { fill:"white" } );
  var labelStyle = new Style( { font: "bold 40px", color:"black" } );
    
  Handler.bind("/discover", Behavior({
	onInvoke: function(handler, message){
	  deviceURL = JSON.parse(message.requestText).url;
	}
  }));
    
  Handler.bind("/forget", Behavior({
	onInvoke: function(handler, message){
	  deviceURL = "";
	}
  }));
    
  var counterLabel = new Label({left:0, right:0, height:40, string:"0", style: labelStyle});
  var ResetButton = BUTTONS.Button.template(function($){ return{
	left: 0, right: 0, height:50,
	contents: [
	  new Label({left:0, right:0, height:40, string:"Reset", style: labelStyle})
	],
	behavior: Object.create(BUTTONS.ButtonBehavior.prototype, {
	  onTap: { value: function(content){
 	    content.invoke(new Message(deviceURL + "reset"), Message.JSON);
	  }},
	  onComplete: { value: function(content, message, json){
	    counterLabel.string = json.count;
	  }}
	})
  }});
    
  var mainColumn = new Column({
	left: 0, right: 0, top: 0, bottom: 0, active: true, skin: whiteSkin,
	contents: [
	  new Label({left:0, right:0, height:40, string:"Counter:", style: labelStyle}),
	  counterLabel,
	  new ResetButton()
	],
	behavior: Behavior({
	  onTouchEnded: function(content){
 	    if (deviceURL != "") content.invoke(new Message(deviceURL + "getCount"), Message.JSON);
	  },
	  onComplete: function(content, message, json){
	    counterLabel.string = json.count;
  	  }	
	})
  });
    
  var ApplicationBehavior = Behavior.template({
	onDisplayed: function(application) {
	  application.discover("tutorial3device.app");
	},
	onQuit: function(application) {
	  application.forget("tutorial3device.app");
	},
  })
    
  application.behavior = new ApplicationBehavior();
  application.add(mainColumn);
</code></pre>
    
There's a lot to take in here! Let's first quickly see at what the result should look like:

<iframe width="500" height="375" src="https://www.youtube.com/embed/5IgfdAfMnaI" frameborder="0" allowfullscreen=""></iframe>

The device app uses only techniques we've already learned earlier in this tutorial or in previous tutorials. It binds two Handlers, one to return the value of a counter (after incrementing it) and another to reset the counter. Both Handlers return JSON data to the invoker to indicate the current count. Additionally, the device shows the current count in a label.

The phone side has become somewhat more complicated. We now store the device URL when it is discovered and implement the "/forget" **Handler** to be notified and delete our stored URL when device apps disappear. Note that this technique would not work well if there were multiple device applications on the network at once — in that case, we would need to maintain a server table as in [this sample](https://github.com/Kinoma/KPR-examples/tree/master/discovery-client).

Next we build up a user interface as we did in [Tutorial 1](../mobile-apps-in-kinoma-studio/) and [Tutorial 2](../asynchronous-communication-in-kinomajs/). In the reset button's behavior we send `Messages` to the device application's "/reset" Handler and verify the response. We also add an `onTouchEnded` event to our mainColumn in order to send `Messages` to the device application's "/getCount" Handler. We process the response to that Message in `onComplete` to update the display.