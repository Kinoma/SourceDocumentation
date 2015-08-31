KinomaJS has a variety of ways to specify color and transparency, which are based on the variety of methods available in CSS. This tech note provides details about each supported method.

***
##Specifying CSS Colors in KinomaJS
As stated throughout the documentation, specifying color in KinomaJS is modeled after CSS3, which supports several ways to specify color. In the following examples, we'll set define a `style` with a purple `color`.

***
##By Basic Color Keyword
First are the CSS3 [Basic Color Keywords](http://www.w3.org/TR/css3-color/#html4):

	var purpleStyle = new Style({color:"purple"});
	var purpleStyle = new Style("purple");
</span>

	<style id="purpleStyle" color="purple">
	
These keywords are as follows: `aqua`, `black`, `blue`, `fuchsia`, `gray`, `green`, `lime`, `maroon`, `navy`, `olive`, `purple`, `red`, `silver`, `teal`, `white`, and `yellow`. The `orange` keyword included in [CSS2's list of basic color keywords](http://www.w3.org/TR/CSS2/syndata.html#value-def-color) was not supported in early releases but has since been added.

The keyword "`transparent`" is also supported, and is in fact the default used when colors are unspecified or invalid.

The [Extended Color Keywords](http://www.w3.org/TR/css3-color/#svg-color) are not currently supported.

The following code examples are shown in JavaScript (with a shorthand version, if available) and in the optional XML markup form.

***
##By Hexadecimal Notation
[Hexadecimal Notation](http://www.w3.org/TR/css3-color/#numerical) is the traditional web color specification, preceded by # and with each successive pair of hexadecimal digits representing the 0-255 values for red, green, and blue, respectively:
	
	var purpleStyle = new Style({color:"#FF00FF"});
	var purpleStyle = new Style("#FF00FF");
</span>

	<style id="purpleStyle" font="24px Arial" color="#FF00FF">

***
##Transparency using Hex Notation
Unique to KinomaJS, there is a method for specifying opacity (or alpha) in hex notation, in aRGB format. It can be represented here by simply prepending another hex pair that represents the level of opacity from 0-255. In the following example, `7F` makes it semitransparent (127 decimal, or 50%).

	var transparentPurpleStyle = new Style({color:"#7FFF00FF"});
	var transparentPurpleStyle = new Style("#7FFF00FF");
</span>
	
	<style id="transparentPurpleStyle" font="24px Arial" color="#7FFF00FF">
	
***
##By Functional Notation
You can also specify color as a [rgb()](http://www.w3.org/TR/css3-color/#rgb-color) function with integers between 0 and 255 for red, green and blue:

	var purpleStyle = new Style({color:"rgb(50%,0%,50%)"});
	var purpleStyle = new Style("rgb(255,0,255)");
</span>

	<style id="purpleStyle" font="24px Arial" color="rgb(255,0,255)">
	
You can even specify percentages, so you don't need to write a conversion function:

	var purpleStyle = new Style({font:"24px Arial",color:"rgb(50%,0%,50%)");
</span>

	<style id="purpleStyle" font="24px Arial" color="rgb(50%,0%,50%)">
	
You can also add alpha-channel transparency using [rgba()](http://www.w3.org/TR/css3-color/#rgba-color) function notation. The alpha must be a value between zero and one, and will be rounded to two decimal places.

	var transparentPurpleStyle = new Style({color:"rgba(255,0,255,0.5)"});
	var transparentPurpleStyle = new Style("rgba(255,0,255,0.5)");
</span>

	<style id="transparentPurpleStyle" font="24px Arial" color="rgba(255,0,255,0.5)">
	
Finally, we can use the CSS3 [hsl()](http://www.w3.org/TR/css3-color/#hsl-color) function notation. The first value, the hue angle, is in degrees: 0 to 360 inclusive. The second and third values are saturation and lightness, respectively, and are expressed as percentages. Like `rgb()`, it supports the alpha channel using `hsla()`, with an transparency value between zero and one, which is internally rounded to two places.

	var purpleStyle = new Style({color:"hsl(270,100%,50%)"});
	var transparentPurpleStyle = new Style({color:"hsla(270,100%,50%,0.66)"});

	var purpleStyle = new Style("hsl(270,100%,50%)");
	var transparentPurpleStyle = new Style("hsla(270,100%,50%,0.66)");
</span>

	<style id="purpleStyle" font="24px Arial" color="hsl(270,100%,50%)">
	<style id="transparentPurpleStyle" font="24px Arial" color="hsla(270,100%,50%,0.66)">

***
##Pitfalls
These CSS color specifications are to be passed as **strings** and the runtime parses them as such. If you are changing them programmatically, be sure to build it as a string:

	var myShade;
	for (var i; i<256; i=i+5) {
		myShade = "rgb(" + i + "," + i + "," + i + ")";
		myButton.color=myShade;
	}
	
The following code **will not work**. While it is valid JavaScript code, it is not a string so it will not be properly recognized, and the result will be **transparent**.

	myShade = rgb(i,i,i)
	
Also, KinomaJS is currently not as forgiving about whitespace in specifiers as web browsers are. Tabs, for example, cannot always be used to line up long lists of color specifications in RGB notation; thay may be considered invalid and return `transparent`.

	
