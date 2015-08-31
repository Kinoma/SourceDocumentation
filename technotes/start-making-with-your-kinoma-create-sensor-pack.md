The Kinoma Create Sensor Pack contains 3 sensors from [sparkfun.com](https://www.sparkfun.com/):

* [AT407 Digital Tilt Sensor](https://www.sparkfun.com/products/10289)
* [ADXL335 Triple Axis Accelerometer](https://www.sparkfun.com/products/9269)
* [TMP102 Digital Temperature Sensor](https://www.sparkfun.com/products/11931)

It is easy to get each of these working with your Kinoma Create. This tutorial walks you through connecting each sensors to your Kinoma Create.

For the Digital Tilt sensor and Triple Axis Accelerometer we will show you how to read values using the [Pin Explorer app](http://blog.kinoma.com/2015/01/exploring-pin-explorer/).

For the Digital Temperature Sensor we will load a Kinoma Create sample into [Kinoma Studio](../../../), customize it, and run on your Kinoma Create.

***
##AT407 Digital Tilt Sensor
The AT407 tilt sensor is a simple digital (on/off) sensor that indicates the orientation of the part. You can incorporate it into a project of your own design in the same way as any other [Digital Input](../../../documentation/tutorials/Digital/). It is easiest to appreciate, however, using the Pin Explorer app. We cover that process here.

![](http://www.kinoma.com/develop/documentation/technotes/images/sensor_pack/sensor_pack_clip_image002.png)

####Step 1: Setup your front pins.
We want to hook up the tilt sensor to the front of Kinoma Create. To make it work, we need to tell the Kinoma Create what pins you will be plugging in where. We do this using the Front Pins app from the home screen. Once in the app, configure your pins as shown below, with one power pin and one digital input:

![](http://www.kinoma.com/develop/documentation/technotes/images/sensor_pack/sensor_pack_clip_image004.png)

Press Back to apply the settings and return to the home screen:

####Step 2: Plug in the sensor
Connect the tilt sensor's two legs to the two pins you just configured. In our example above, this is pins 53 and 54, the 3rd and 4th pins from the left on the left-front header. It doesn't matter which leg goes in which socket.

![](http://www.kinoma.com/develop/documentation/technotes/images/sensor_pack/tilt-closeup.jpg)

####Step 3: Run Pin Explorer
Launch the Pin Explorer app by tapping its icon on the Kinoma Create home screen.

![](http://www.kinoma.com/develop/documentation/technotes/images/sensor_pack/apply-mux.png)

####Step 4: Select your Digital Input and watch the results!
Once in Pin Explorer, select your Digital Input (front pin 54) from the pin list at the bottom of the screen. You should see the graph changing when you turn your device upside-down. You will hear the small metal ball in the tilt sensor rolling – after that the display will update to show that your sensor is outputting a new value. Note that you may have to tape down the sensor to keep it in.

***
##ADXL335 Triple Axis Accelerometer

![](http://www.kinoma.com/develop/documentation/technotes/images/sensor_pack/sensor_pack_clip_image012.png)

(Photo courtesy of SparkFun Electronics)

The ADXL335 triple axis accelerometer communicates with Kinoma Create via analog inputs. It can be used to drive several of our [Analog Input](../../../documentation/tutorials/AnalogIn/) samples, including the [Analog Skeleton](../../../documentation/tutorials/analog-skeleton/) and the [Analog Value Graph](../../../documentation/tutorials/analog-value-graph/). It is also the sensor used to monitor for shaking the device in the [Drawing Toy](../../../documentation/tutorials/analog-drawing-toy/) sample.

####Step 1: Setup your Front Pins
As with the tilt sensor, we first need to configure our front pins for the sensor. In this case, we need three analog inputs, a ground pin, and a 3.3v power pin. Your front-pin configuration should look like this:

![](http://www.kinoma.com/develop/documentation/technotes/images/sensor_pack/sensor_pack_clip_image014.png)

####Step 2: Plug in the Sensor
Next we plug in the sensor to the pins we just configured. We align the far-left pin (ST) of the sensor with the far-left of the front-left header. This will put the ST pin in Pin 51, the analog inputs on Pins 52-54, ground on Pin 55, and V+ on Pin 56.

![](http://www.kinoma.com/develop/documentation/technotes/images/sensor_pack/adxl-closeup.jpg)

####Step 3: Verify that Your Sensor is Working in Pin Explorer
We use the Pin Explorer app again to verify that we're getting useful data back from our sensor. After that, we proceed to use or customize one of the Analog In samples.

After launching Pin Explorer, select Pin 53 at the bottom of the screen. The values graphed correspond to the Y axis of the sensor. If you shake your device front to back, you will see the graph move in a corresponding way.

![](http://www.kinoma.com/develop/documentation/technotes/images/sensor_pack/sensor_pack_clip_image016.png)

***
##TMP102 Digital Temperature Sensor

![](http://www.kinoma.com/develop/documentation/technotes/images/sensor_pack/sensor_pack_clip_image018.png)

(Photo courtesy of SparkFun Electronics)

The TMP102 digital temperature sensor measures temperatures on the board and communicates them to Kinoma Create using the I2C digital interface. We have [a sample](../../../documentation/tutorials/i2c-temperature/) that uses this sensor to display the temperature (note that the registered temperature is often be hotter than ambient due to the sensor itself getting warm). We could also retrieve the temperature value using Pin Explorer, though that is not done here.

####Step 1: Setup your Front Pins
Once again, we need to configure the front pins to accept the TMP102 sensor. In this case, we need a 3.3v power, a ground, an I2C data, and an I2C clock. Your configuration should look like this:

![](http://www.kinoma.com/develop/documentation/technotes/images/sensor_pack/sensor_pack_clip_image020.png)

####Step 2: Plug in the Sensor
Next we plug in the sensor using the pins we just configured. Left-align the sensor in the front-left header. This puts the power pin on Pin 51, ground on Pin 52, SDA on Pin 53, and SCL on Pin 54.

![](http://www.kinoma.com/develop/documentation/technotes/images/sensor_pack/temp-close.jpg)

####Step 3: Load the i2c-temperature Sample in Kinoma Studio
(If you do not have Kinoma Studio installed, you can download it from [http://kinoma.com/develop/studio](http://kinoma.com/develop/studio))

The first time you run Kinoma Studio, you will see the Welcome Screen, which looks like this:

![](http://www.kinoma.com/develop/documentation/technotes/images/sensor_pack/sensor_pack_clip_image022.png)

You can load sample code by clicking the “Examples” button. Do so, and then scroll down to the “I2C Temperature” sample. Click it and then hit “OK” to install the I2C Temperature project in your Kinoma Studio workspace.

![](http://www.kinoma.com/develop/documentation/technotes/images/sensor_pack/sensor_pack_clip_image024.png)

The I2C Temperature project appears in Kinoma Studio in your Project Explorer on the left:

![](http://www.kinoma.com/develop/documentation/technotes/images/sensor_pack/sensor_pack_clip_image026.png)

####Step 4: Run the Project in a Simulator
Before running the project on your Kinoma Create, it is interesting to first try it out on your computer. You can do this from the application.xml file, which should have opened by default when you loaded the project. If not, you can open it simply by double clicking application.xml in the Project Explorer.

From application.xml, click Run next to the Kinoma Create simulator line:

![](http://www.kinoma.com/develop/documentation/technotes/images/sensor_pack/sensor_pack_clip_image028.png)

The resulting simulator will look like this:

![](http://www.kinoma.com/develop/documentation/technotes/images/sensor_pack/sensor_pack_clip_image030.png)

You can move the slider on the left side of the screen to see the result of different sensor inputs. This control is a Hardware Pin Simulator, which you can learn more about in [this Kinoma Tech Note](../../../documentation/technotes/pins-simulators.php).

####Step 5: Edit the Sample for Your Pin Selection
This sample is setup by default as if you were connecting your TMP102 sensor to the back of the Kinoma Create device. Since we, instead, connected to the front of the device we'll need to make a quick edit to the source code.

Open main.xml in the src directory in Project Explorer by double clicking.

![](http://www.kinoma.com/develop/documentation/technotes/images/sensor_pack/sensor_pack_clip_image032.png)

The change you need to make is simple. Look for this code:

	//Set up I2C bll, creating tmpSensor pins object. 
	application.invoke( new MessageWithObject("pins:configure", { 
		tmpSensor: { 
			require: "TMP102", 
			pins: { 
				temperature: { sda: 27, clock: 29 } 
			} 
		} 
	}));
	
You just need to change "`sda: 27`" to "`sda: 53`" and "`clock: 29`" to "`clock: 54`" to match the pin configuration we selected on Kinoma Create earlier.

Save your changes, and then you're ready to proceed.

####Step 6: Run to the Kinoma Create
You are now prepared to run your app to your Kinoma Create. You can do this by tapping the run button next to your Kinoma Create from application.xml. Note that your Kinoma Create will have whatever name you have setup using the Settings app on the device. Note also that for this to work, your computer and your Kinoma Create must be on the same Wi-Fi network.

![](http://www.kinoma.com/develop/documentation/technotes/images/sensor_pack/sensor_pack_clip_image034.png)

You should now see the same app from the Simulator running on your Kinoma Create! You can try out the temperature sensor by placing your finger across it, causing it to heat up or cool down, depending on its internal temperature.

***
##Conclusion
Congratulations! You have now used three new sensors with your Kinoma Create. You can continue to learn from here by picking up some more sensors to play with, or by building your own custom app using these sensors. To learn more about building apps in [Kinoma Studio](../../../), please check out our [Getting Started webinar]
(https://www.youtube.com/watch?v=EkOPeOad24I&feature=youtu.be), [Documentation](../../../documentation), and [App Development in Kinoma Studio tutorial](../../../documentation/technotes/mobile-apps-in-kinoma-studio/).

Thank you for your purchase and please enjoy your Kinoma Create!