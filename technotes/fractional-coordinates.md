This tech note shows how fractional pixel coordinates can be used in KinomaJS to create smooth animations, especially for slow moving objects.

***
##Background
All [Content Objects](http://kinoma.com/develop/documentation/javascript/#_Toc275330434") in KinomaJS can be resized or moved, and some can be rotated and skewed.  To create animations you simply adjust these properties at specified intervals.

A graphical object in KinomaJS has coordinates measured in logical pixels to determine where it’s placed on the screen.  If you want an object to move quickly from one side of the screen to another, you can shift it over several pixels every interval and our eyes will fill in the blanks and see it as if it’s hitting every coordinate on the way over.  All numbers are stored as floating point numbers in JavaScript, but KPR rounds numbers used as coordinates to integers by default.  This isn’t generally problematic; you won’t be able to tell the difference between an object moving exactly 4 pixels and moving 4.0001 pixels.  However it may be undesirable if you want to, for example, translate an image by a tenth of a pixel at a time in an animation to make it move very slowly.  The good news is you can explicitly enable the use of fractional coordinates which will allow you to create slow and steady movement.  The next sections will tell you when and how you can do this.

***
##Fractional Coordinates in KinomaJS
In KinomaJS, [Layer Objects](http://kinoma.com/develop/documentation/javascript/#_Toc275330440") and [Picture Objects](http://kinoma.com/develop/documentation/javascript/#_Toc275330445) have a **subPixel** property that, when set to true, allows you to give an object fractional coordinates.  In many projects the difference is not noticeable or doesn’t exist, but if you want  to animate with small movements, slow motion, or smooth scaling it may be necessary.  It’s easy to check if it does make a difference—all you have to do is set the **subPixel** property to true—and a couple of quick examples to demonstrate its usefulness can be found below.

***
##Exercises
####Slideshow Project
The [Slideshow sample project](https://github.com/Kinoma/KPR-examples/tree/master/slideshow) is a great example of where fractional coordinates are necessary.  Download the source code and run the application.  You will see a series of images with Ken Burns effects applied to them.

Now open up main.xml.  If you read through the code you’ll see that it’s creating animation by adjusting the scale and translation of the images shown.  It’s a few hundred lines of code, but you only have to edit one.  Go to line 54 and change

	picture.subPixel = true;
to

	picture.subPixel = false;


Now run the application again.  Note that the animation is no longer as smooth as it was before—images seem to twitch a bit several times a second.  Comparing the two side by side makes the difference quite obvious.

<iframe width="890" height="500" src="https://www.youtube.com/embed/i2sYa4vUxYQ?rel=0&amp;vq=hd720" frameborder="0" allowfullscreen></iframe>

####Simple Layer Translation

If you want a simpler JavaScript example to work with, try the following exercise.  This application animates Layer objects with white squares and text on them.

Start by creating a behavior prototype that makes a layer move slowly to the right.

	var LayerBehavior = function () {
		this.dx = 0;
	}
	LayerBehavior.prototype = Object.create(Object.prototype, {
		onDisplaying: {
			value: function(layer) {
				layer.start();
			}
		},
		onTimeChanged: {
			value: function(layer) {
				this.dx += 0.1;
				layer.translation = {x: this.dx, y: 0};
			}
		},
	});

Create a Layer object with a white box and some text.  Set its **subPixel** property to true.

	var labelStyle = new Style( { font: "40px", color:"white" } );
	var whiteSkin = new Skin({fill:"white"});
	var subpixelLayer = new Layer({left:0, right:0, top:15, height:100, 
		contents: [
			new Content({top:0, left:1, width:100, height:100, skin: whiteSkin}),
			new Label({top:0, bottom:0, left:110, width:200, string:"subpixel", style:labelStyle,})
		]
	})
	subpixelLayer.subPixel = true;

Create another Layer object with the same content.  Keep its **subPixel** property unchanged (making it default to false).

	var pixelLayer = new Layer({left:0, right:0, top:125, height:100, 
		contents: [
			new Content({top:0, left:1, width:100, height:100, skin: whiteSkin}),
			new Label({top:0, bottom:0, left:110, width:200, string:"pixel", style:labelStyle,})
		]
	})

Set their behaviors and add them to the application.

	subpixelLayer.behavior = new LayerBehavior();
	pixelLayer.behavior = new LayerBehavior();
	application.add(subpixelLayer);
	application.add(pixelLayer);
	application.skin = new Skin({fill:"black"});

You should see that the text that uses fractional coordinates goes across the screen smoothly, while the normal text twitches as it goes across. Here’s a video of the simulator:

<iframe width="890" height="500" src="https://www.youtube.com/embed/iimwPI1Bk3A?rel=0&amp;vq=hd720" frameborder="0" allowfullscreen></iframe>

Now go back to the line in the behavior prototype that adds to the dx value. Change it to add 0.5 instead of 0.1.

	this.dx += 0.5;

If you run the application again you’ll see something that the difference between the two strings’ animation is much less pronounced.  A side by side comparison can be seen in the video below.

<iframe width="890" height="500" src="https://www.youtube.com/embed/uz7TDiaK3Vc?rel=0&amp;vq=hd720" frameborder="0" allowfullscreen></iframe>

***
##When to Use Fractional Coordinates
If you spend some time changing parameters in the sample code above you’ll see that using fractional coordinates only makes a noticeable difference when you scale or translate an object very slowly, making it unnecessary for many projects.  For example if you’re animating an object using the moveBy() function, there’s no reason to use them since moveBy will round the deltas inputted to the nearest integer.  

Rendering with fractional coordinates may be slower, so if there’s no visible difference it is not recommended.  But, as you’ve now seen, it certainly makes a difference for projects that require small, subtle, or slow animation and can greatly improve a project.

***
##Drawbacks
You may have noticed that objects whose **subPixel** property is set to true look slightly out of focus.  To understand why it’s best to look at what is happening in slow motion.  In the video below I zoomed in and slowed the animation down to 10% speed.  Watch it in full screen mode and you’ll see that the edges of the square aren’t actually moving at each interval; they’re simply cycling through shades of gray.  This is what causes the blurring effect. 

<iframe width="890" height="500" src="https://www.youtube.com/embed/doAf6At0NCw?rel=0&amp;vq=hd720" frameborder="0" allowfullscreen></iframe>

***
##Additional Note
While writing this tech note I came across a strange quirk.  If you look at the code that adds the white squares to each layer you’ll notice that the Content objects have a 1-pixel left margin. If you change this to zero and run the application you’ll see that the left side of the square in the subpixel layer twitches like the square in the normal layer.  

This is caused by rounding that occurs when the image is rendered.  It will inevitably occur anytime a Layer object’s contents have a left/right margin of 0 and move horizontally or a top/bottom margin of 0 and move vertically, so be on the lookout for that when writing your own project and add a small margin to mask the effect.