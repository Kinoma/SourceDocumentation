This tutorial will teach you to interact with webservices using KinomaJS. It assumes a basic understanding of how to use Kinoma Studio and how to build grid-based user interfaces with KinomaJS. If you are not familar with these topics, please refer to the [Mobile Apps in Kinoma Studio Quick Start Guide](../mobile-apps-in-kinoma-studio/index.php).

***
##Getting Started

Create a new Application Project with a JavaScript main file (main.js) as per [these instructions](../mobile-apps-in-kinoma-studio/index.php#newProject).

Throughout this tutorial, we will be building towards an app that displays your device's public IP address and the time. Both of these pieces of information will be pulled from the public web services provided by [JSON Test](http://www.jsontest.com/).

The final result will look like this:

![](../images/AsynchronousComm/finalResult.jpg)

***
##Building the UI

We'll start by building the basic user interface. Copy and paste this code into your main.js:

	var whiteSkin = new Skin({fill:"white"});

	var titleStyle = new Style({font:"bold 70px", color:"black"});
	var resultStyle = new Style({font:"45px", color:"black"});

	var mainColumn = new Column({
		left: 0, right: 0, top: 0, bottom: 0,
		skin: whiteSkin,
		contents:[
			new Label({left: 0, right: 0, height: 70, string: "Your IP:", style: titleStyle}),
			new Label({left: 0, right: 0, height: 70, string: "Loading...", style: resultStyle}),
			new Label({left: 0, right: 0, height: 70, string: "Time:", style: titleStyle}),
			new Label({left: 0, right: 0, height: 70, string: "Loading...", style: resultStyle}),
		]
	});

	application.behavior = Object.create(Behavior.prototype, {
		onLaunch: { value: function(application, data){
			application.add(mainColumn);
		}}
	});
	
There is one new technique used here, as compared to the previous tutorial: an application behavior. Here, we override the onLaunch event of the application to add our mainColumn to the application's layout. The result will look like this if you run it in the iPhone simulator:

![](../images/AsynchronousComm/justTheUI.jpg)

***
##Calling Methods Asynchronously via Messages & Handlers

Thus far, all the programmatic elements we've seen in KinomaJS applications have been triggered directly in response to events (e.g. onTouchEnded). But there are other ways to organize the flow of your application's logic. For an in-depth discussion of this topic, see the **Flow section** of the [KinomaJS Overview](../../overview/).

If you have background tasks to complete (such as a lengthy computation or waiting for a response from a web service) it is important that you execute them [asynchronously](http://www.i-programmer.info/programming/theory/6040-what-is-asynchronous-programming.html) to avoid locking up the user interface and the rest of your app during execution. The mechanisms for doing this in KinomaJS are Messages and Handlers. Messages asynchronously ask another part of your program or an external webservice to complete some task. Handlers listen for Messages addressed to them, can execute code, can send new Messages of their own, and can wait for responses to Messages.

When a Message is sent using the path that a Handler is bound to, that Handler's onInvoke function will be called asynchronously. Here, we add two Handlers to our application and send Messages to trigger them from our application's onLaunch:

	var whiteSkin = new Skin({fill:"white"});

	var titleStyle = new Style({font:"bold 70px", color:"black"});
	var resultStyle = new Style({font:"45px", color:"black"});

	var mainColumn = new Column({
		left: 0, right: 0, top: 0, bottom: 0,
		skin: whiteSkin,
		contents:[
			new Label({left: 0, right: 0, height: 70, string: "Your IP:", style: titleStyle}),
			new Label({left: 0, right: 0, height: 70, string: "Loading...", style: resultStyle, name:"ipLabel"}),
			new Label({left: 0, right: 0, height: 70, string: "Time:", style: titleStyle}),
			new Label({left: 0, right: 0, height: 70, string: "Loading...", style: resultStyle, name:"timeLabel"}),
		]
	});

	Handler.bind("/getIP", {
		onInvoke: function(handler, message){
			trace("You invoked getIP\n");
		},
	});

	Handler.bind("/getTime", {
		onInvoke: function(handler, message){
			trace("You invoked getTime\n");
		},
	});

	application.behavior = Object.create(Behavior.prototype, {
		onLaunch: { value: function(application, data){
    		application.add(mainColumn);
    		application.invoke(new Message("/getIP"));
    		application.invoke(new Message("/getTime"));
		}}
	});
	
This will trace two lines to the console showing your IP address and the current time (in UTC):

`Returned IP is: 24.130.211.117`

`Returned Time is: 10:16:50 AM`

***
##Modifying The UI Programmatically

When the results come back from your web service call, you may want to update your user interface in response. The easiest way to do this is by using the **name** property of a piece of content. This will help you find a specific sub-content of a piece of content. You can then make the change directly on the content.

To complete our sample, we add names to the two Labels in our UI. We then take the results that come back from our web service calls and use them to change the string property of the appropriate Label. This will replace the default "Loading..." text with the proper result.

	var whiteSkin = new Skin({fill:"white"});

	var titleStyle = new Style({font:"bold 70px", color:"black"});
	var resultStyle = new Style({font:"45px", color:"black"});

	var mainColumn = new Column({
		left: 0, right: 0, top: 0, bottom: 0,
		skin: whiteSkin,
		contents:[
			new Label({left: 0, right: 0, height: 70, string: "Your IP:", style: titleStyle}),
			new Label({left: 0, right: 0, height: 70, string: "Loading...", style: resultStyle, name:"ipLabel"}),
			new Label({left: 0, right: 0, height: 70, string: "Time:", style: titleStyle}),
			new Label({left: 0, right: 0, height: 70, string: "Loading...", style: resultStyle, name:"timeLabel"}),
		]
	});

	Handler.bind("/getIP", {
		onInvoke: function(handler, message){
			handler.invoke(new Message("http://ip.jsontest.com/"), Message.JSON);
		},
		onComplete: function(handler, message, json){
			mainColumn.ipLabel.string = json.ip;
		}
	});

	Handler.bind("/getTime", {
		onInvoke: function(handler, message){
			handler.invoke(new Message("http://time.jsontest.com/"), Message.JSON);
		},
		onComplete: function(handler, message, json){
			mainColumn.timeLabel.string = json.time;
		}
	});

	application.behavior = Object.create(Behavior.prototype, {
		onLaunch: { value: function(application, data){
			application.add(mainColumn);
			application.invoke(new Message("/getIP"));
			application.invoke(new Message("/getTime"));
		}}
	});

This gives us the final form of our app, which looks like this:

![](../images/AsynchronousComm/finalResult.jpg)
